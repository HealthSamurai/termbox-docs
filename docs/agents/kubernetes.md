# Roll Out to Kubernetes

**Goal:** deploy Termbox on Kubernetes, or roll out a new image or content version without downtime.

**Ask the user first:**

- Which cluster context and namespace to use.
- The PostgreSQL host, database, user and password. Use a managed or existing instance.
- Which content to load. Start from the `data.yaml` in [Start Termbox Locally](start-local.md).
- The license to use. The license in Start Termbox Locally is for development and evaluation. Production needs a [production license](../licensing.md).

Show the user the changes and get confirmation before applying them to a shared cluster.

## 1. Create the config and secrets

Keep `data.yaml` in a ConfigMap, so that changing it is a reviewable rollout:

```bash
kubectl -n <namespace> create configmap termbox-data --from-file=data.yaml
kubectl -n <namespace> create secret generic termbox-secrets \
  --from-literal=PG_PASSWORD='<password>' \
  --from-literal=LICENSE='<license>'
```

## 2. Apply the Deployment

Save this as `termbox.yaml`, fill in the placeholders, and run `kubectl -n <namespace> apply -f termbox.yaml`.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: termbox
spec:
  replicas: 1
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 0
      maxSurge: 1
  selector:
    matchLabels:
      app: termbox
  template:
    metadata:
      labels:
        app: termbox
    spec:
      terminationGracePeriodSeconds: 30
      initContainers:
        - name: fetch-resources
          image: curlimages/curl:8.10.1
          command:
            - sh
            - -c
            - mkdir -p /data/resources && curl -fsSL -o /data/resources/UCUM.json https://storage.googleapis.com/termbox-public/resources/UCUM.json
          volumeMounts:
            - name: data-files
              mountPath: /data
      containers:
        - name: termbox
          image: healthsamurai/termbox:1.0.2  # use the newest version from Release Notes
          ports:
            - containerPort: 3000
          env:
            - name: PG_HOST
              value: <postgres-host>
            - name: PG_DATABASE
              value: <postgres-database>
            - name: PG_USER
              value: <postgres-user>
            - name: PG_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: termbox-secrets
                  key: PG_PASSWORD
            - name: LICENSE
              valueFrom:
                secretKeyRef:
                  name: termbox-secrets
                  key: LICENSE
            - name: DATA_CONFIG_FILE
              value: /config/data.yaml
            - name: READONLY_MODE
              value: "true"
            - name: ADMIN_API_ENABLED
              value: "false"
            - name: LOG_FORMAT
              value: json
          readinessProbe:
            httpGet:
              path: /fhir/metadata
              port: 3000
            initialDelaySeconds: 10
            periodSeconds: 10
          lifecycle:
            preStop:
              exec:
                command: ["sh", "-c", "sleep 15"]
          volumeMounts:
            - name: data-files
              mountPath: /data
              readOnly: true
            - name: termbox-data
              mountPath: /config
              readOnly: true
      volumes:
        - name: data-files
          emptyDir: {}
        - name: termbox-data
          configMap:
            name: termbox-data
---
apiVersion: v1
kind: Service
metadata:
  name: termbox
spec:
  selector:
    app: termbox
  ports:
    - port: 80
      targetPort: 3000
```

`READONLY_MODE` turns off FHIR write endpoints, and `ADMIN_API_ENABLED` turns off the unauthenticated `/admin` API. Content still loads from `data.yaml` at startup. The `fetch-resources` init container downloads the files that `data.yaml` reads from `/data`. Add one `curl` per extra resource or package, using the URLs from [Find Terminologies in the Gallery](gallery.md).

## 3. Roll out a change

To change the image, update `image` and apply the file. To change content, pin the new versions in `data.yaml`, update the ConfigMap, and restart:

```bash
kubectl -n <namespace> create configmap termbox-data --from-file=data.yaml --dry-run=client -o yaml | kubectl -n <namespace> apply -f -
kubectl -n <namespace> rollout restart deployment/termbox
kubectl -n <namespace> rollout status deployment/termbox
```

The new pod starts before the old one stops. They share the same database, so the old content keeps serving while the new content loads.

## 4. Verify

`rollout status` doesn't wait for content to load, so check the content itself:

```bash
kubectl -n <namespace> port-forward service/termbox 3000:80 &
curl -s -G 'http://localhost:3000/fhir/CodeSystem/$lookup' --data-urlencode 'system=http://loinc.org' --data-urlencode 'code=8867-4'
```

Repeat until `version` is the one you expect, then stop the port forward with `kill %1`. To follow progress, run `kubectl -n <namespace> logs deployment/termbox -f`.

## 5. Report to the user

Give the image, the versions now loaded, and how the service can be reached. This manifest has no Ingress, so exposing it publicly is a separate step.

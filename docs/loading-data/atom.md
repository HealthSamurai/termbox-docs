# Atom Feed

Loads from a syndication feed.

| Field                     | Optional | Description                                                                                      |
| ------------------------- | -------- | ------------------------------------------------------------------------------------------------ |
| `feed`                    |          | URL of the syndication feed                                                                      |
| `auth`                    | yes      | Authentication configuration                                                                     |
| `auth.type`               |          | Auth strategy; only `client_credentials` is supported                                            |
| `auth.token_url`          |          | OAuth2 token endpoint                                                                            |
| `auth.client_id`          |          | Client ID                                                                                        |
| `auth.client_secret`      | yes      | Plaintext client secret                                                                          |
| `auth.client_secret_env`  | yes      | Environment variable containing the client secret                                                |
| `auth.client_secret_file` | yes      | Path to a file containing the client secret                                                      |
| `include`                 | yes      | List of filters; only entries matching any filter are loaded                                     |
| `include.url`             |          | Canonical URL to match                                                                           |
| `include.version`         | yes      | Version pattern to match (supports [wildcards](https://build.fhir.org/references.html#matching)) |
| `exclude`                 | yes      | List of filters; entries matching any filter are skipped                                         |
| `exclude.url`             |          | Canonical URL to match                                                                           |
| `exclude.version`         | yes      | Version pattern to match (supports [wildcards](https://build.fhir.org/references.html#matching)) |

```yaml
sources:
  - type: atom
    feed: https://ontology.nhs.uk/production1/synd/syndication.xml
```

## Authentication

Feeds that require authentication use OAuth2 client credentials. Use one of `client_secret`, `client_secret_env`, or `client_secret_file` to provide the secret.

```yaml
sources:
  - type: atom
    feed: https://ontology.nhs.uk/production1/synd/syndication.xml
    auth:
      type: client_credentials
      token_url: https://ontology.nhs.uk/authorisation/auth/realms/nhs-digital-terminology/protocol/openid-connect/token
      client_id: your-client-id
      client_secret: your-client-secret
```

Or load from an environment variable:

```yaml
sources:
  - type: atom
    feed: https://ontology.nhs.uk/production1/synd/syndication.xml
    auth:
      type: client_credentials
      token_url: https://ontology.nhs.uk/authorisation/auth/realms/nhs-digital-terminology/protocol/openid-connect/token
      client_id: your-client-id
      client_secret_env: NHS_CLIENT_SECRET
```

Or load from a local file:

```yaml
sources:
  - type: atom
    feed: https://ontology.nhs.uk/production1/synd/syndication.xml
    auth:
      type: client_credentials
      token_url: https://ontology.nhs.uk/authorisation/auth/realms/nhs-digital-terminology/protocol/openid-connect/token
      client_id: your-client-id
      client_secret_file: /run/secrets/nhs-client-secret
```

## Filtering feed entries

Large syndication feeds can contain hundreds of entries. Use `include` and `exclude` to select only the entries you need.

- **`include`**: when present, an entry is loaded only if it matches at least one filter in the list.
- **`exclude`**: an entry is skipped if it matches at least one filter in the list.

Each filter must contain a `url`, and optionally, a `version`. The `version` field supports wildcard matching as defined in the [FHIR specification](https://build.fhir.org/references.html#matching): for example, `http://snomed.info/sct/83821000000107` matches any release of the SNOMED UK edition, and `1.0.*` matches any patch of a pinned semver minor.

{% hint style="warning" %}
YAML may parse bare decimal values such as `4.0` as a number. Quote version strings that look like decimals to avoid this.
{% endhint %}

### Include

The example below loads only the SNOMED UK Edition and ICD-10 UK version 4.0 from the NHS feed:

```yaml
sources:
  - type: atom
    feed: https://ontology.nhs.uk/production1/synd/syndication.xml
    auth:
      type: client_credentials
      client_id: MyClientId
      client_secret_env: NHS_SECRET
      token_url: https://ontology.nhs.uk/authorisation/auth/realms/nhs-digital-terminology/protocol/openid-connect/token
    include:
      - url: http://snomed.info/sct
        version: http://snomed.info/sct/83821000000107
      - url: http://hl7.org/fhir/sid/icd-10-uk
        version: '4.0'
```

### Exclude

To load everything except a specific entry, use `exclude` instead:

```yaml
sources:
  - type: atom
    feed: https://ontology.nhs.uk/production1/synd/syndication.xml
    auth:
      type: client_credentials
      client_id: MyClientId
      client_secret_env: NHS_SECRET
      token_url: https://ontology.nhs.uk/authorisation/auth/realms/nhs-digital-terminology/protocol/openid-connect/token
    exclude:
      - url: http://hl7.org/fhir/sid/icd-10-uk
```

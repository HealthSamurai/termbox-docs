# Licensing

Termbox requires a valid license before it can be used for serving FHIR API requests. If Termbox starts without a valid license, an error message is logged indicating that a license is required. You will see a log like:

```log
ERROR LOG 2a5cddb56506 termbox.license[106,20] License required. Acquire license: http://localhost:3000/ui/license
```

Similarly, if you send a request to the FHIR API, you will receive an error response like:

```json
# HTTP/1.1 403 Forbidden
{
  "resourceType": "OperationOutcome",
  "issue": [
    {
      "code": "forbidden",
      "severity": "error",
      "diagnostics": "License required. Acquire license: http://localhost:3000/ui/license"
    }
  ]
}
```

Both messages describe the way to acquire a license. Simply navigate to the indicated URL and choose an option. After acquiring a license, you are ready to start using the FHIR API. Termbox currently offers two license types: **Community** and **Professional Edition**.

## Community

The community license is free and intended for:

- Development and Evaluation
- Research & academic use
- Non-commercial projects

To get a community license, simply follow the steps described in `/ui/license` page. Create an account in Health Samurai if you don't have one or sign-in if you do. You will be automatically redirected back to termbox and the _License required_ message will disappear. You can start using the FHIR API.

## Professional Edition

The **Professional Edition** license is intended for:

- Commercial use
- Production environments

To get a **Professional Edition** license, contact Health Samurai to discuss your use case and make a commercial agreement.

## The `LICENSE` Env Var

Additionally, you can provide a license key directly via environment variable instead of using the UI. The key is issued by Health Samurai as part of a **Professional Edition** license agreement. This is the recommended way to run Termbox in production environments.

```bash
export LICENSE="WAIdt5WQJzFA9gyF6oQishsoOxaA8iEPJQw..."
```

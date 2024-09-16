# Helm chart of [oauth2-proxy](https://github.com/oauth2-proxy/oauth2-proxy) using OIDC provider

The repository contains a helm chart that can be used to deploy [oauth2-proxy](https://github.com/oauth2-proxy/oauth2-proxy) configured to use `OIDC provider` for user authentication.

## Configuration

```yaml
ingress:
    host: # Host name of ingress object, consider that a new ingress object will be created to handle a request to /oauth2 path.
 
# OIDC provider configuration
oidc:
    clientId: # Client ID

    clientSecret: # Client secret

    cookieSecret: # How to generate cookie secret you can find here https://oauth2-proxy.github.io/oauth2-proxy/configuration/overview#generating-a-cookie-secret

    issuerUrl: # Issuer URL

    # Additional command line arguments for oauth2-proxy,
    # more information you can find here https://oauth2-proxy.github.io/oauth2-proxy/configuration/overview#command-line-options
    extraArgs: []
```

More configuration options can be found in [chart/values.yaml](./chart/values.yaml).

## Example

Following example reflects configuration used to deploy `oauth2-proxy` configured to use [Hashicorp Vault](https://www.vaultproject.io/) `OIDC provider`:

```yaml
ingress:
  host: example.com
oidc:
  clientId: 3XGHWI6gaONw9DMRqER6dcGRDZnxCwXs
  clientSecret: hvo_secret_T6lWmnnpdtpeLOWS3QX0tk3PzMGhrM4WbvJ9IygE6W3q0USsVsOsfUKuZ8MqZOo6
  cookieSecret: TOw1sHArkcOYi4Ler85-ogL-ZYmkklc2AahcuBPVT1s=
  extraArgs:
  - --set-authorization-header=true
  - --email-domain="*"
  - --cookie-refresh=1m
  - --cookie-expire=30m
  - --code-challenge-method=S256
  - --scope=openid user groups
  issuerUrl: https://[vault base URL]/v1/identity/oidc/provider/[provider name]
```

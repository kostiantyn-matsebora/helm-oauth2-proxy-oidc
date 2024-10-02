# Simple OAuth2 Proxy Helm Chart

Simplified helm chart for deploying and configuring [oauth2-proxy] provides a way to configure `OIDC provider` as any other OAuth2 provider supported by application.

## Configuration

The following table lists the main configurable parameters of the oauth2-proxy chart and their default values.

```yaml

auth:
  # OIDC configuration, all fields are required.
  # Following command line arguments are used to configure OIDC provider.
  oidc:
    enabled: false
    # Set it true if you are using Keycloak as OIDC provider.
    isKeyCloak: false
    # Issuer URL
    # For Authentik application looks like https://[authentik base uri]/application/o/[application name]/
    # For Hashicorp Vault looks like https://[vault url]/v1/identity/oidc/provider/[provider name]
    issuerUrl: 

  # Configuration  passed to toml file
  config:
    # OIDC application Client ID. Required for OAuth2 providers.
    clientId: 
    # OID application  Client Secret. Required for OAuth2 providers.
    clientSecret:
    # OAuth2 scope to request. Required for OAuth2 providers.
    scope:
    # Cookie secret. Required. https://oauth2-proxy.github.io/oauth2-proxy/docs/configuration/overview/#config-file
    cookieSecret:
    # Set Authorization Bearer response header (useful in Nginx auth_request mode). Default false.
    setAuthorizationHeader:
    #  Authenticate emails with the specified domain (may be given multiple times).
    #  Use * to authenticate any email. Default empty.
    emailDomain:
    # Refresh the cookie after this duration; 0 to disable; not supported by all providers. Default empty
    cookieRefresh:
    # Expire the cookie after this duration; 0 to expire with browser session; not supported by all providers. Default 168h0m0s
    cookieExpire:
    # use PKCE code challenges with the specified method. Either 'plain' or 'S256' (recommended). Default empty
    codeChallengeMethod:

# Extra configuration added to toml configuration file, more information you can find here https://oauth2-proxy.github.io/oauth2-proxy/configuration/overview,
# Example: 
# extraConfig: |
#   cookie_domain = "example.com"
#   cookie_path = "/"
#   ...
#
# Consider using it if you can use it to define any other provider than OIDC
  extraConfig:

# Name of the runtime class to use for this pod https://kubernetes.io/docs/concepts/containers/runtime-class/  
runtimeClassName:
```

More configuration options can be found in [chart/values.yaml](./chart/values.yaml).

## Example

Following example reflects configuration used to deploy [oauth2-proxy] configured to use [Hashicorp Vault](https://www.vaultproject.io/) `OIDC provider`:

```yaml
ingress:
  host: example.com
auth:
  oidc:
    enabled: true
    issuerUrl: https://[vault base URL]/v1/identity/oidc/provider/[provider name]
  config:
    clientId: 3XGHWI6gaONw9DMRqER6dcGRDZnxCwXs
    clientSecret: hvo_secret_T6lWmnnpdtpeLOWS3QX0tk3PzMGhrM4WbvJ9IygE6W3q0USsVsOsfUKuZ8MqZOo6
    cookieSecret: TOw1sHArkcOYi4Ler85-ogL-ZYmkklc2AahcuBPVT1s=
    setAuthorizationHeader: true
    emailDomain: example.com
    cookieRefresh: 1m 
    cookieExpire: 30m
    codeChallengeMethod: S256
    scope: openid user groups
  extraConfig: |
     cookie_name = "_grandmas_delicious_cookie"
```

## Usage

Add helm [repository](https://kostiantyn-matsebora.github.io/helm-charts/) first:

```bash
helm repo add kostiantyn-matsebora https://kostiantyn-matsebora.github.io/helm-charts/
```

Install/upgrade helm chart using your custom values (for instance storage-provisioner):

```bash

# oauth2-proxy
helm upgrade oauth2-proxy kostiantyn-matsebora/simple-oauth2-proxy --install --values ./custom-values.yaml

```

## Contributing

If you experience any issues, have a question or a suggestion, or if you wish
to contribute, feel free to [open an issue][issues] or
[start a discussion][discussions].

[issues]: https://github.com/kostiantyn-matsebora/oauth2-proxy-simple/issues
[discussions]: https://github.com/kostiantyn-matsebora/oauth2-proxy-simple/discussions

## License

[`MIT License`](../LICENSE)


[oauth2-proxy]:https://github.com/oauth2-proxy/oauth2-proxy

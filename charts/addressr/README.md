# addressr Helm Chart

Helm chart for addressr.

## Usage

### Quick Guide

Validate the chart:

`helm lint .`

Dry run and print out rendered YAML:

`helm install --dry-run --debug addressr .`

Dry run and print out rendered YAML with merged values file:

```
helm install \
  --dry-run \
  --debug \
  -f helm-values.local.yaml \
    addressr .
```

#### Installation

`helm install addressr .`

Or, with some different values:

```
helm install addressr \
  --set image.tag="latest" \
  --set service.type="LoadBalancer" \
    .
```

Or, the same but with a custom values from a file:

```
helm install addressr \
  -f helm-values.local.yaml \
    .
```

Example values file (this is `dev-stockpay`):

```
ingress:
  enabled: true
  className: "kong"
  annotations:
    kubernetes.io/tls-acme: "true"
    cert-manager.io/cluster-issuer: letsencrypt-staging
  hosts:
    - host: api.dev.stockpay.io
      paths:
        - path: /
          pathType: ImplementationSpecific
  tls:
  - secretName: api-dev-stockpay-io-tls
    hosts:
    - api.dev.stockpay.io
```

#### Testing

Testing after creation of a release:

`helm test .`

#### Upgrading

Upgrade the chart, with values file:

```
helm upgrade addressr . \
  -f helm-values.local.yaml
```

#### Uninstallation

Completely remove the chart:

`helm uninstall addressr`

## Testing

Test the different endpoints.

Within cluster:
```
curl -v http://addressr:8080/v1/addresses/?q=LEVEL+25,+TOWER+3
curl http://addressr.dev-stockpay:8080/
curl http://addressr-elasticsearch:9200/
curl http://addressr-elasticsearch.dev-stockpay:9200/
```
Public:
```
curl http://api.dev.stockpay.io/v1/addresses/?q=LEVEL+25,+TOWER+3
```

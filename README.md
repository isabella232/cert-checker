# cert-checker

[![Build Status](https://img.shields.io/endpoint.svg?url=https://actions-badge.atrox.dev/mogensen/go-git-open/badge)](https://actions-badge.atrox.dev/mogensen/cert-checker/goto)
[![Go Report Card](https://goreportcard.com/badge/github.com/mogensen/cert-checker)](https://goreportcard.com/report/github.com/mogensen/cert-checker)
[![codecov](https://codecov.io/gh/mogensen/cert-checker/branch/master/graph/badge.svg)](https://codecov.io/gh/mogensen/cert-checker)
[![FOSSA Status](https://app.fossa.com/api/projects/git%2Bgithub.com%2Fmogensen%2Fcert-checker.svg?type=shield)](https://app.fossa.com/projects/git%2Bgithub.com%2Fmogensen%2Fcert-checker?ref=badge_shield)

cert-checker is a certificate monitoring utility for watching tls certificates. These
checks get exposed as Prometheus metrics to be viewed on a dashboard, or _soft_
alert cluster operators.

This tool is heavily inspired by the awesome [version-checker by jetstack](https://github.com/jetstack/version-checker/).

## Registries

cert-checker supports the following types of certificate errors (and possible more):

- Expired certificates
- Wrong host
- Bad root certificates
- Revoked certificate
- Cipher suites not allowed
    * dh480
    * dh512
    * null
    * rc4

---

## Installation

cert-checker can be installed as a standalone static binary from the release page

[latest release](https://github.com/mogensen/cert-checker/releases/latest/)

### In Kubernetes as static manifests 

cert-checker can be installed as static manifests:

```sh
$ kubectl apply -k ./deploy/yaml
```

Remember to edit the configmap with the actual domains you want to monitor.. 

## Metrics

By default, cert-checker will expose the version information as Prometheus
metrics on `0.0.0.0:8080/metrics`.


## Development

Test the full setup in Kubernetes with Prometheus and Grafana dashboards:

```bash
# First create a new kind cluster locally, and install prometheus
make dev.kind.create
# Build a docker image, load it into kind and deploy cert-checker and promeheus/grafana stuff
make image dev.kind.install
```

Access the local infrastructure here:

| System        | URL                                                                                                        |
| ------------- |------------------------------------------------------------------------------------------------------------|
| Prometheus    | http://prometheus.localtest.me/graph?g0.expr=cert_checker_is_valid&g0.tab=1&g0.stacked=0&g0.range_input=1h | 
| Grafana       | http://grafana.localtest.me/d/cert-checker/certificate-checker                                             | 


---
title: Monitoring
---

# Monitoring of Packit Service

## Pushgateway

To record _Packit Service_-related metrics from Celery tasks we are going to use
[Prometheus Pushgateway](https://github.com/prometheus/pushgateway) which is
[deployed](https://github.com/packit/deployment/blob/main/openshift/pushgateway.yml.j2) in our cluster.
It can collect the metrics from the workers and provide the `/metrics` endpoint for Prometheus.
There is a Prometheus instance running in OpenShift PSI, which is going to
scrape the `/metrics` endpoint and then it will be possible
to visualize them. Therefore the `/metrics` endpoint needs to be publicly
accessible - it is exposed on https://workers.packit.dev/metrics for metrics
of the production instance and https://workers.stg.packit.dev/metrics
for metrics of the staging instance.
We use nginx ([definition](https://github.com/packit/deployment/blob/main/openshift/nginx.yml.j2)) to serve as a reverse
proxy for the pushgateway, which enables us to allow only `GET` requests and
forward these to pushgateway (workers can send `POST` requests internally).

## Flower

To record _celery_ related metrics from Celery tasks we are going to use
[Flower Prometheus Integration](http://mher.github.io/flower/prometheus-integration.html#prometheus-integration).

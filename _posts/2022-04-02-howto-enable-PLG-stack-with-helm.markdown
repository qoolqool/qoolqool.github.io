---
layout: post
title:  "Howto enable Prometheus, Loki, Grafana stack using Helm Chart"
date:   2022-04-02 15:00:00 +0800
categories: [k8s,grafana]
---
### Requirement
To enable Prometheus, Loki, Grafana stack using official Helm Chart

### Component
- helm
- kubernetes
- grafana
- loki

#### Procedure
1) Get official help chart from Grafana

    ```
    helm repo add grafana https://grafana.github.io/helm-charts
    ```

2) Update helm repository

    ```
    helm repo update
    ```

3) Enable PLG stack

    ```
    helm upgrade --install loki --namespace=monitoring grafana/loki-stack  --set grafana.enabled=true,prometheus.enabled=true,prometheus.alertmanager.persistentVolume.enabled=false,prometheus.server.persistentVolume.enabled=false
    ```

4) Verify charts

    ```
    helm list -A
    NAME	NAMESPACE 	REVISION	UPDATED                                	STATUS  	CHART           	APP VERSION
    loki	monitoring	1       	2022-03-26 14:36:13.243779398 +0000 UTC	deployed	loki-stack-2.6.1	v2.1.0
    ```

### Limitation
n/a

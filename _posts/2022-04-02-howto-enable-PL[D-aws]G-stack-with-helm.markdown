---
layout: post
title:  "Howto enable Prometheus, Loki-distributed with AWS S3 storage, Grafana stack using Helm Chart"
date:   2022-04-08 22:38:00 +0800
categories: [k8s,grafana]
---
### Requirement
- To enable Promtail, Loki-distributed, Grafana using official Helm Chart.
- To configure AWS S3 for loki-distributed storage

### Component
- helm
- kubernetes
- grafana
- loki
- Promtail
- AWS S3

#### Logical Diagram
![](https://raw.githubusercontent.com/qoolqool/qoolqool.github.io/main/static/img/_posts/logical.png)

#### Procedure
1) Get official helm chart from Grafana

  ```
  helm repo add grafana https://grafana.github.io/helm-charts
  ```

2) Update helm repository

  ```
  helm repo update
  ```

3) AWS S3 bucket preparation
  ![](https://raw.githubusercontent.com/qoolqool/qoolqool.github.io/main/static/img/_posts/awss3.png)

4) Enable Promtail
  - Prepare values files (Get the default values file from official helm chart)
    - Edit the `lokiAddress`

    ```
    lokiAddress: http://loki-loki-distributed-distributor:3100/loki/api/v1/push
    ```

  - Execute the helm command
    ```
    helm upgrade --install promtail -f values-promtail.yml grafana/promtail -n monitoring
    Release "promtail" has been upgraded. Happy Helming!
    NAME: promtail
    LAST DEPLOYED: Fri Apr  8 14:56:19 2022
    NAMESPACE: monitoring
    STATUS: deployed
    REVISION: 5
    TEST SUITE: None
    NOTES:
    ***********************************************************************
     Welcome to Grafana Promtail
     Chart version: 3.11.0
     Promtail version: 2.4.2
    ***********************************************************************

    Verify the application is working by running these commands:

    * kubectl --namespace monitoring port-forward daemonset/promtail 3101
    * curl http://127.0.0.1:3101/metrics
    ```

5) Enable Loki distributed
  - Prepare values files (Get the default values file from official helm chart)
    - Add the storage_config to the `structuredConfig` section

  ```
  structuredConfig:
    storage_config:
      boltdb_shipper:
        active_index_directory: /var/loki/index
        cache_location: /var/loki/cache
        cache_ttl: 168h         # Can be increased for faster performance over longer query periods, uses more disk space
        shared_store: s3
      aws:
        bucketnames: qool-loki
        s3: s3://ap-southeast-1
        access_key_id: <YOURID>
        secret_access_key: <YOURSECRET>
  ```
  - Execute the helm command
    ```
    $ helm upgrade --install loki -f d-values.yml grafana/loki-distributed -n monitoring
    Release "loki" does not exist. Installing it now.
    NAME: loki
    LAST DEPLOYED: Fri Apr  8 14:35:37 2022
    NAMESPACE: monitoring
    STATUS: deployed
    REVISION: 1
    TEST SUITE: None
    NOTES:
    ***********************************************************************
     Welcome to Grafana Loki
     Chart version: 0.46.4
     Loki version: 2.4.2
    ***********************************************************************

    Installed components:
    * ingester
    * distributor
    * querier
    * query-frontend
    ```
6) Enable Grafana

  ```
  helm upgrade --install grafana  grafana/grafana -n monitoring
  W0408 15:01:56.874588   67010 warnings.go:70] policy/v1beta1 PodSecurityPolicy is deprecated in v1.21+, unavailable in v1.25+
  W0408 15:01:56.878364   67010 warnings.go:70] policy/v1beta1 PodSecurityPolicy is deprecated in v1.21+, unavailable in v1.25+
  W0408 15:01:56.892659   67010 warnings.go:70] policy/v1beta1 PodSecurityPolicy is deprecated in v1.21+, unavailable in v1.25+
  W0408 15:01:56.901269   67010 warnings.go:70] policy/v1beta1 PodSecurityPolicy is deprecated in v1.21+, unavailable in v1.25+
  W0408 15:01:56.905010   67010 warnings.go:70] policy/v1beta1 PodSecurityPolicy is deprecated in v1.21+, unavailable in v1.25+
  W0408 15:01:56.910255   67010 warnings.go:70] policy/v1beta1 PodSecurityPolicy is deprecated in v1.21+, unavailable in v1.25+
  Release "grafana" has been upgraded. Happy Helming!
  NAME: grafana
  LAST DEPLOYED: Fri Apr  8 15:01:56 2022
  NAMESPACE: monitoring
  STATUS: deployed
  REVISION: 2
  NOTES:
  1. Get your 'admin' user password by running:

     kubectl get secret --namespace monitoring grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo

  2. The Grafana server can be accessed via port 80 on the following DNS name from within your cluster:

     grafana.monitoring.svc.cluster.local

     Get the Grafana URL to visit by running these commands in the same shell:

       export POD_NAME=$(kubectl get pods --namespace monitoring -l "app.kubernetes.io/name=grafana,app.kubernetes.io/instance=grafana" -o jsonpath="{.items[0].metadata.name}")
       kubectl --namespace monitoring port-forward $POD_NAME 3000

  3. Login with the password from step 1 and the username: admin
  #################################################################################
  ######   WARNING: Persistence is disabled!!! You will lose your data when   #####
  ######            the Grafana pod is terminated.                            #####
  #################################################################################
  ```
  - Add data source in grafana
  ![](https://raw.githubusercontent.com/qoolqool/qoolqool.github.io/main/static/img/_posts/grafana-ds.png)

7) Verify charts
  ```
  helm list -A
  NAME    	NAMESPACE 	REVISION	UPDATED                                	STATUS  	CHART                  	APP VERSION
  grafana 	monitoring	1       	2022-04-04 15:16:51.020409997 +0000 UTC	deployed	grafana-6.24.1         	8.4.2
  loki    	monitoring	1       	2022-04-08 14:35:37.668550622 +0000 UTC	deployed	loki-distributed-0.46.4	2.4.2
  promtail	monitoring	5       	2022-04-08 14:56:19.192495877 +0000 UTC	deployed	promtail-3.11.0        	2.4.2
  ```
### Limitation
n/a

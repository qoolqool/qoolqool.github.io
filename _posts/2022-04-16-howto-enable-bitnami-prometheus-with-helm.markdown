---
layout: post
title:  "Howto enable Bitnami Prometheus Operator using Helm Chart"
date:   2022-04-16 17:18:00 +0800
categories: [k8s]
---
### Requirement
- To enable kubernetes cluster observability with minimum effort

### Component
- helm
- kubernetes
- prometheus


#### Procedure
1) Get official helm chart from Bitnami

  ```
  helm repo add bitnami https://charts.bitnami.com/bitnami
  ```

2) Update helm repository

  ```
  helm repo update
  ```
3) Enable Bitnami Prometheus Operator

  - Execute the helm command
    ```
    helm install prometheus bitnami/kube-prometheus -n monitoring
    NAME: prometheus
    LAST DEPLOYED: Sat Apr  9 00:39:25 2022
    NAMESPACE: monitoring
    STATUS: deployed
    REVISION: 1
    TEST SUITE: None
    NOTES:
    CHART NAME: kube-prometheus
    CHART VERSION: 6.9.2
    APP VERSION: 0.55.1

    ** Please be patient while the chart is being deployed **

    Watch the Prometheus Operator Deployment status using the command:

        kubectl get deploy -w --namespace monitoring -l app.kubernetes.io/name=kube-prometheus-operator,app.kubernetes.io/instance=prometheus

    Watch the Prometheus StatefulSet status using the command:

        kubectl get sts -w --namespace monitoring -l app.kubernetes.io/name=kube-prometheus-prometheus,app.kubernetes.io/instance=prometheus

    Prometheus can be accessed via port "9090" on the following DNS name from within your cluster:

        prometheus-kube-prometheus-prometheus.monitoring.svc.cluster.local

    To access Prometheus from outside the cluster execute the following commands:

        echo "Prometheus URL: http://127.0.0.1:9090/"
        kubectl port-forward --namespace monitoring svc/prometheus-kube-prometheus-prometheus 9090:9090

    Watch the Alertmanager StatefulSet status using the command:

        kubectl get sts -w --namespace monitoring -l app.kubernetes.io/name=kube-prometheus-alertmanager,app.kubernetes.io/instance=prometheus

    Alertmanager can be accessed via port "9093" on the following DNS name from within your cluster:

        prometheus-kube-prometheus-alertmanager.monitoring.svc.cluster.local

    To access Alertmanager from outside the cluster execute the following commands:

        echo "Alertmanager URL: http://127.0.0.1:9093/"
        kubectl port-forward --namespace monitoring svc/prometheus-kube-prometheus-alertmanager 9093:9093
    ```

7) Verify chart Resources

  ```
  $ kubectl get all --all-namespaces -l='app.kubernetes.io/managed-by=Helm,app.kubernetes.io/instance=prometheus'
  NAMESPACE    NAME                                                                READY   STATUS    RESTARTS   AGE
  monitoring   pod/prometheus-kube-prometheus-blackbox-exporter-86d4c556d5-dxzcz   1/1     Running   0          6d2h
  monitoring   pod/prometheus-kube-prometheus-operator-7988f76c57-pdjvk            1/1     Running   0          6d2h
  monitoring   pod/prometheus-kube-state-metrics-fb8d664b4-v578w                   1/1     Running   0          6d2h
  monitoring   pod/prometheus-node-exporter-lg6mv                                  1/1     Running   0          7d8h
  monitoring   pod/prometheus-node-exporter-mwvz7                                  1/1     Running   0          7d8h
  monitoring   pod/prometheus-node-exporter-xw8l7                                  1/1     Running   0          7d8h

  NAMESPACE     NAME                                                         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)     AGE
  kube-system   service/prometheus-kube-prometheus-coredns                   ClusterIP   None            <none>        9153/TCP    7d8h
  kube-system   service/prometheus-kube-prometheus-kube-controller-manager   ClusterIP   None            <none>        10252/TCP   7d8h
  kube-system   service/prometheus-kube-prometheus-kube-proxy                ClusterIP   None            <none>        10249/TCP   7d8h
  kube-system   service/prometheus-kube-prometheus-kube-scheduler            ClusterIP   None            <none>        10251/TCP   7d8h
  monitoring    service/prometheus-kube-prometheus-alertmanager              ClusterIP   10.233.57.97    <none>        9093/TCP    7d8h
  monitoring    service/prometheus-kube-prometheus-blackbox-exporter         ClusterIP   10.233.42.24    <none>        19115/TCP   7d8h
  monitoring    service/prometheus-kube-prometheus-operator                  ClusterIP   10.233.35.242   <none>        8080/TCP    7d8h
  monitoring    service/prometheus-kube-prometheus-prometheus                ClusterIP   10.233.22.126   <none>        9090/TCP    7d8h
  monitoring    service/prometheus-kube-state-metrics                        ClusterIP   10.233.32.83    <none>        8080/TCP    7d8h
  monitoring    service/prometheus-node-exporter                             ClusterIP   10.233.24.27    <none>        9100/TCP    7d8h

  NAMESPACE    NAME                                      DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
  monitoring   daemonset.apps/prometheus-node-exporter   3         3         3       3            3           <none>          7d8h

  NAMESPACE    NAME                                                           READY   UP-TO-DATE   AVAILABLE   AGE
  monitoring   deployment.apps/prometheus-kube-prometheus-blackbox-exporter   1/1     1            1           7d8h
  monitoring   deployment.apps/prometheus-kube-prometheus-operator            1/1     1            1           7d8h
  monitoring   deployment.apps/prometheus-kube-state-metrics                  1/1     1            1           7d8h

  NAMESPACE    NAME                                                                      DESIRED   CURRENT   READY   AGE
  monitoring   replicaset.apps/prometheus-kube-prometheus-blackbox-exporter-86d4c556d5   1         1         1       7d8h
  monitoring   replicaset.apps/prometheus-kube-prometheus-operator-7988f76c57            1         1         1       7d8h
  monitoring   replicaset.apps/prometheus-kube-state-metrics-fb8d664b4                   1         1         1       7d8h

  NAMESPACE    NAME                                                                    READY   AGE
  monitoring   statefulset.apps/alertmanager-prometheus-kube-prometheus-alertmanager   1/1     7d8h
  monitoring   statefulset.apps/prometheus-prometheus-kube-prometheus-prometheus       1/1     7d8h
  ```

8) Verify CRD

  ```
  $ kubectl get crd | grep coreos
  alertmanagerconfigs.monitoring.coreos.com             2022-04-09T00:36:11Z
  alertmanagers.monitoring.coreos.com                   2022-04-09T00:36:11Z
  podmonitors.monitoring.coreos.com                     2022-04-09T00:36:11Z
  probes.monitoring.coreos.com                          2022-04-09T00:36:11Z
  prometheuses.monitoring.coreos.com                    2022-04-09T00:36:11Z
  prometheusrules.monitoring.coreos.com                 2022-04-09T00:36:11Z
  servicemonitors.monitoring.coreos.com                 2022-04-09T00:36:11Z
  thanosrulers.monitoring.coreos.com                    2022-04-09T00:36:11Z
  ```

### Limitation
n/a

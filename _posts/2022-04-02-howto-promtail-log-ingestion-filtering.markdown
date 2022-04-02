---
layout: post
title:  "Howto configure Promtail Scrape Config to achieve log ingestion filtering"
date:   2022-04-02 23:00:00 +0800
categories: [k8s,grafana]
---
### Requirement
To control log ingestion filtering using Promtail

### Component
- Promtail

#### Procedure
1) Add the following snippet to promtail configmap (Assuming loki stack spin up in kubernetes)

  ```
    scrape_configs:
    # Define job name
    - job_name: syslog
    # Define log source statically
      static_configs:
      - targets:
          - localhost
    # Define log path (will be captured as metadata)
        labels:
          __path__: /var/log/pods/*/promtail/*.log
    # Define job name as metadata
          job: syslog
    # Define instruction for this specific job
      pipeline_stages:
    # Define keep instruction with match based on regex
      - match:
          selector: '{job="syslog"}'
          stages:
          - regex:
              expression: '.*level=(?P<level>[a-zA-Z]+).*ts=(?P<syslog_datetime>[T\d-:.Z]*).*component=(?P<keep>[a-zA-Z]+).*'
          - labels:
              level:
              keep:
          action: keep
      # Define drop instruction when the "label-keep" does not contain anything
      - match:
          selector: '{job="syslog", keep!~"(.+)"}'
          action: drop
  ```

2) Restart Promtail Pod

```
kubectl rollout restart daemonset/loki-promtail -n monitoring
```

3) Verify result in Grafana


### Limitation
n/a

---
layout: post
title:  "Howto setup rsyslog to forward log to promtail based on specific file"
date:   2022-04-16 17:37:00 +0800
categories: [linux,grafana]
---
### Requirement
- To forward log to promtail as syslog receiver with rsyslog as syslog forwarder
- Based on specific file and log pattern

### Component
- rsyslog
- promtail

#### Procedure
1) Setup syslog receiver in Promtail (Under scrape config)

  ```
  - job_name: syslog
    syslog:
      listen_address: 0.0.0.0:1514
      idle_timeout: 60s
      label_structured_data: yes

    relabel_configs:
      - source_labels: ['__syslog_message_hostname']
        target_label: 'hostname'
      - source_labels: ['__syslog_message_app_name']
        target_label: 'appname'
      - source_labels: ["__syslog_message_severity"]
        target_label: "severity"
      - source_labels: ["__syslog_message_facility"]
        target_label: "facility"
    pipeline_stages:
    - match:
        selector: '{appname="testlog"}'
        stages:
        - regex:
            expression: '.*protocol=(?P<protocol>[a-zA-Z]+).*'
        - labels:
            protocol:
        action: keep
    - match:
        selector: '{appname="testlog", protocol!~"dns"}'
        action: drop
  ```


2) Setup rsyslog as log forwarder (/etc/rsyslog.d/something.conf)

  ```
  module(load="imfile" PollingInterval="1")
  input(type="imfile" File="/var/log/testlog"
          Tag="testlog:"
  )
  module(load="mmutf8fix")
  action(type="mmutf8fix" replacementChar="?")

  if( ($syslogtag == 'testlog:') )  then {
      action(type="omfwd" Protocol="tcp" Target="promtail.qool.one" Port="1514" Template="RSYSLOG_SyslogProtocol23Format" TCP_Framing="octet-counted")
      stop
  }
  ```


### Limitation
n/a

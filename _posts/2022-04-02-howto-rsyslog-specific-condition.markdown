---
layout: post
title:  "Howto forward syslog entry to remote syslog server with rsyslog"
date:   2022-04-02 12:20:00 +0800
categories: [linux]
---
### Requirement
To sent syslog from specific folder and specific pattern to remote syslog server using rsyslog

### Component
- rsyslog
- systemd
- redhat linux

#### Flow
![](/static/img/_posts/rsyslog-flow.png)

#### Procedure
1) Add a rule file in `/etc/rsyslog.d` with the following content
  - Filename: 50-forward.conf (rsyslog will read any file name with `conf` extension)

    ```
    $ModLoad imfile                       # load file module
    $InputFilePollInterval 1              # file polling every 1 second
    $InputFileName /var/log/secure        # target file
    $InputFileTag secure:                 # user defined tagging
    $InputFileStateFile stat-secure       # stat file for rsyslog to keep track of the forwarding progress
    $InputFileSeverity error              # user defined Severity
    $InputFileFacility local3             # user defined log facility
    $InputRunFileMonitor                  # enable file mointoring
    :syslogtag, isequal, "secure:" {      # filtering based on tagging
      :msg, regex, ".*\\(ls\\|cat\\).*" { # filtering based on syslog message content
        local3.* @syslog.server.local:514 # push syslog to the defined target based on specific syslog facility
      }
      stop                                # discard further processing once condition match
    ```
2) Restart rsyslog Service
  `systemctl restart rsyslog`

### Limitation
n/a

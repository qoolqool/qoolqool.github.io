---
layout: post
title:  "Using ovftool"
date:   2022-04-01 11:40:00 +0800
categories: [vmware]
---
### Requirement
Copy VM between 2 VCenter

### Method
- ovftool

#### Procedure
**Tool Installation - Linux (RHEL)**
- Download from www.vmware.com with valid account
- Install Dependency
    - sudo yum install libnsl
- Install ovftool bundle
    - sudo ./VMware-ovftool-4.4.0-15722219-lin.x86_64.bundle<br>

**Command Execution**
{% highlight ruby %}
ovftool --disableVerification -nw="dst-portgroup-name" -ds="dst-data-store-name" "vi://username@domainname:password@src-vcenter-ip/src-vcenter-name/vm/vm-instance-name" "vi://username@domainname:password@dst-vcenter-ip/dst-vcenter-name/host/dst-cluster-name"
{% endhighlight %}

**Execution Output**
{% highlight ruby %}
Opening VI source: vi://username%40domainname@vcenter-ip:443/src-vcenter-name/vm/src-vm-instance-name
Opening VI target: vi://cusername%4domainname@dst-vcenter-ip:443/dst-vcenter-name/host/dst-cluster-name
Deploying to VI: vi://cusername%4domainname@dst-vcenter-ip:443/dst-vcenter-name/host/dst-cluster-name
Disk progress: 4%
Disk progress: 17%

Transfer Completed
Completed successfully
{% endhighlight %}

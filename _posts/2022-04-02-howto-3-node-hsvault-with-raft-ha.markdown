---
layout: post
title:  "Howto create 3-node HSVault with raft"
date:   2022-04-02 12:06:00 +0800
categories: [security,hashicorp]
---
### Requirement
To setup 3-node Hashicorp Vault solution with Raft HA on TLS communication channel

### Method
- HS Vault
- openssl
- raft HA

#### Flow
![image](https://user-images.githubusercontent.com/64636576/147900978-015802a0-073b-47d2-9e92-108ba212b922.png)


#### Procedure
1) Download & install HSVault according to official instruction
  - [How to install Hashicorp Vault](https://learn.hashicorp.com/tutorials/vault/getting-started-install)

2) Create TLS key & TLS cert with CA Cert (self-signed/authorized)
  - [How to Create SAN Certificate](https://github.com/qoolqool/howto/blob/main/HSVault/san-cert.md)

3) Upload all TLS key, cert and CA cert to HSVault Nodes with proper file ownership and permission
  - Copy file to the following folder with the specific permission to all 3 nodes
  ```
  /opt/vault/tls
  -rw-r--r--. root root  ca.pem
  -rw-r--r--. root root  tls.crt
  -rw-r-----. root vault tls.key
  ```

4) Ensure Vault FQDN DNS resolution works
  - Create A record in DNS or add an entry in /etc/hosts

5) Configure Vault & Raft
   > cluster_addr & leader_tls_servername must be FQDN and defined in alternate names of the SAN TLS Certificate

  - Node1
    ```
    cluster_addr  = "https://vault01.int.qool.one:8201"
    api_addr      = "https://vault.int.qool.one:8200"
    disable_mlock = true

    listener "tcp" {
      address            = "0.0.0.0:8200"
      tls_cert_file      = "/opt/vault/tls/tls.crt"
      tls_key_file       = "/opt/vault/tls/tls.key"
      tls_client_ca_file      = "/opt/vault/tls/ca.pem"

    }

    storage "raft" {
      path    = "/opt/vault/data"
      node_id = "vault01"

      retry_join {
        leader_tls_servername   = "vault01.int.qool.one"
        leader_api_addr         = "https://vault01.int.qool.one:8200"
        leader_ca_cert_file     = "/opt/vault/tls/ca.pem"
        leader_client_cert_file = "/opt/vault/tls/tls.crt"
        leader_client_key_file  = "/opt/vault/tls/tls.key"
      }
      retry_join {
        leader_tls_servername   = "vault02.int.qool.one"
        leader_api_addr         = "https://vault02.int.qool.one:8200"
        leader_ca_cert_file     = "/opt/vault/tls/ca.pem"
        leader_client_cert_file = "/opt/vault/tls/tls.crt"
        leader_client_key_file  = "/opt/vault/tls/tls.key"
      }
      retry_join {
        leader_tls_servername   = "vault03.int.qool.one"
        leader_api_addr         = "https://vault03.int.qool.one:8200"
        leader_ca_cert_file     = "/opt/vault/tls/ca.pem"
        leader_client_cert_file = "/opt/vault/tls/tls.crt"
        leader_client_key_file  = "/opt/vault/tls/tls.key"
      }
    }

    ui = true
    ```
  - Node2 & Node3
    ```
    Similar to Node1. Just replace
      - cluster_addr to node specific FQDN defined in DNS
      - leader_tls_servername to node specific FQDN defined in DNS
    ```
6) Setup Environment on all 3 nodes
   ```
   export VAULT_CACERT=/opt/vault/tls/ca.pem
   ```

7) Initialize Vault on Primary HSVault Node
  - Execute the following command on Primary HSVaule Node. ie: vault01
    ```
    vault operator init
    ```

8) Unseal all 3 HSVault Node
   ```
   vault operator unseal
   ```

### Limitation

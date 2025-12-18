**agent based config file**
```
apiVersion: v1beta1
kind: AgentConfig
metadata:
  name: arbiter

# Master-1 IP acts as bootstrap (rendezvous)
rendezvousIP: 190.170.20.73

additionalNTPSources:
  - 190.170.20.113

hosts:
  - hostname: rhcos-master1-arbiter.lab.kuberox.net
    role: master
    interfaces:
      - name: ens33
        macAddress: 00:50:56:85:4e:34
    rootDeviceHints:
      deviceName: /dev/nvme0n1
    networkConfig:
      interfaces:
        - name: ens33
          type: ethernet
          state: up
          mac-address: 00:50:56:85:4e:34
          ipv4:
            enabled: true
            address:
              - ip: 190.170.20.73
                prefix-length: 23
            dhcp: false
      dns-resolver:
        config:
          server:
            - 190.170.20.113

  - hostname: rhcos-master2-keshav.lab.kuberox.net
    role: master
    interfaces:
      - name: ens33
        macAddress: 00:50:56:85:83:0e
    rootDeviceHints:
      deviceName: /dev/nvme0n1
    networkConfig:
      interfaces:
        - name: ens33
          type: ethernet
          state: up
          mac-address: 00:50:56:85:83:0e
          ipv4:
            enabled: true
            address:
              - ip: 190.170.20.74
                prefix-length: 23
            dhcp: false
      dns-resolver:
        config:
          server:
            - 190.170.20.113

  - hostname: rhcos-arbiter-keshav.lab.kuberox.net
    role: arbiter
    interfaces:
      - name: ens33
        macAddress: 00:50:56:85:33:e9
    rootDeviceHints:
      deviceName: /dev/nvme0n1
    networkConfig:
      interfaces:
        - name: ens33
          type: ethernet
          state: up
          mac-address: 00:50:56:85:33:e9
          ipv4:
            enabled: true
            address:
              - ip: 190.170.20.75
                prefix-length: 23
            dhcp: false
      dns-resolver:
        config:
          server:
            - 190.170.20.113

```

**Intsall Required Packages**
```
wget https://mirror.openshift.com/pub/openshift-v4/clients/ocp/4.20.0/openshift-client-cp/4.20.0/openshift-install-linux-4.20.0.tar.gz

wget https://mirror.openshift.com/pub/openshift-v4/clients/ocp/4.20.0/openshift-install-linux-4.20.0.tar.gz

tar -xzvf openshift-client-linux-4.20.0.tar.gz
tar -xzvf openshift-install-linux-4.20.0.tar.gz

cp kubectl oc openshift-install /usr/local/bin/
cp kubectl oc openshift-install /usr/local/sbin/

oc version
oc openshift-install version
```

**Add Pull secret + agent file + install config in the same folder**
```
vi install-config.yaml

apiVersion: v1
baseDomain: lab.kuberox.net
metadata:
  name: arbiter-cluster

compute:
- name: worker
  replicas: 0

controlPlane:
  name: master
  replicas: 2

arbiter:
  name: arbiter
  replicas: 1

platform:
  baremetal:
    apiVIP: 190.170.20.80
    ingressVIP: 190.170.20.81

pullSecret: |
  <PASTE CONTENT OF pull-secret.txt HERE>

sshKey: |
  ssh-rsa AAAA....your_ssh_key

```

```
Install nmstate on bastion (RECOMMENDED)

sudo dnf install -y nmstate
```

```
openshift-install agent create image
```


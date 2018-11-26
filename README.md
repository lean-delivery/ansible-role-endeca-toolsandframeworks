endeca_toolsandframeworks
=========
[![License](https://img.shields.io/badge/license-Apache-green.svg?style=flat)](https://raw.githubusercontent.com/lean-delivery/ansible-role-endeca-toolsandframeworks/master/LICENSE)
[![Build Status](https://travis-ci.org/lean-delivery/ansible-role-endeca-toolsandframeworks.svg?branch=master)](https://travis-ci.org/lean-delivery/ansible-role-endeca-toolsandframeworks)
[![Build Status](https://gitlab.com/lean-delivery/ansible-role-endeca-toolsandframeworks/badges/master/build.svg)](https://gitlab.com/lean-delivery/ansible-role-endeca-toolsandframeworks)

## Summary
--------------

This role installs Oracle Endeca Tools and Frameworks on Linux platforms which enables the dynamic presentation of content across all channels.


Requirements
--------------

 - Minimal Version of the ansible for installation: 2.5
 - **Supported ToolsAndFrameworks versions**:
   - 3.x.x
   - 11.x.x
   - _higher versions should be retested_
 - **Supported OS**:
   - CentOS
     - 6
     - 7

For more information regarding support matrix please visit <https://support.oracle.com>

MDEX Engine and PlatformServices should be installed preliminarily:
  - lean_delivery.endeca_mdex
  - lean_delivery.endeca_platformservices

```
For test scenarios endeca_toolsandframeworks/requirements.yml is used  
If another roles/versions are required, put requirements.yml to molecule/<scenario_name> and remove in molecule.yml lines  
  options:  
    role-file: requirements.yml
```


Role Variables
--------------

  - `transport` - artifact source transport  
     Available:
      - `web` - fetch artifact from custom web uri
      - `local` - local artifact

  - `transport_web` - URI for http/https artifact  e.g. "http://my-storage.example.com/V861200-01.zip"
  - `transport_local` - path for local artifact e.g. "/tmp/V861200-01.zip"

  - `download_path` - local folder for downloading artifacts  
    default: `/tmp`

  - `tf_version` - Endeca ToolsAndFrameworks version

#### Set ToolsAndFrameworks version as it's defined in official Oracle Documentation

  - `base_root` - where PlatformServices should be installed  
    default: `/opt`

  - `tf_service` - name of the service  
    default: `endeca-toolsandframeworks`

  - `install_type` - type of installation  
    Available:  
      - `Complete Installation`
      - `Minimal Installation`

  - `nfs_exports` - list of NFS shared folders (to share with ATG Oracle Commerce store application)  
    Example:  

```yaml
      nfs_exports:  
        - "/home/public *(rw,sync,no_root_squash)"
```

#### Parameters for ToolsAndFrameworks versions starting from 11.0

  - `install_group` - install group for user  
    default: `oinstall`

  - `inventory_directory` - path to oracle inventory directory  
    default: `{{ base_root }}/oraInventory`

  - `ora_inst` - path to oraInst.loc file  
    default: `/etc/oraInst.loc`

##### Swap configuration

  - `swapfile_path` - path to swap file  
    default: `/swapfile`

  - `swapfile_bs_size_mb`  
    default: `1`

  - `swapfile_count` - swap size  
    default: `512`

  - `workbench_password` - update default workbench `admin` password (e.g. `Admin123`)

  - `workbench_port` - workbench port  
    default: `8006`

Example Playbook
----------------

### Installing Endeca ToolsAndFrameworks 11.3.0 from local:
```yaml
- name: "Install ToolsAndFrameworks 11.3.0 from local"
  hosts: all

  roles:
    - role: lean_delivery.endeca_mdex
      transport: "local"
      transport_local: "/tmp/V861206-01.zip"
    - role: lean_delivery.endeca_platformservices
      transport: "local"
      transport_local: "/tmp/V861203-01.zip"
    - role: lean_delivery.endeca_toolsandframeworks
      tf_version: "11.3.0"
      transport: "local"
      transport_local: "/tmp/V861200-01.zip"
      workbench_password: "Admin123"
  vars:
    base_root: "/opt"
    mdex_version: "11.3.0"
    ps_version: "11.3.0"
```

### Installing Endeca ToolsAndFrameworks 11.3.0 from web with NFS shared folders:
```yaml
- name: "Install ToolsAndFrameworks 11.3.0 from web with NFS shared folders"
  hosts: all

  roles:
    - role: lean_delivery.endeca_mdex
      transport: "web"
      transport_web: "http://my-storage.example.com/V861206-01.zip"
    - role: lean_delivery.endeca_platformservices
      ps_version: "11.3.0"
      transport: "web"
      transport_web: "http://my-storage.example.com/V861203-01.zip"
    - role: lean_delivery.endeca_toolsandframeworks
      tf_version: "11.3.0"
      transport: "web"
      transport_web: "http://my-storage.example.com/V861200-01.zip"
      workbench_password: "Admin123"
      nfs_exports:
        - "/home/public *(rw,sync,no_root_squash)"
  vars:
    base_root: "/opt"
    mdex_version: "11.3.0"
    ps_version: "11.3.0"
```


## License

[Apache License 2.0](https://raw.githubusercontent.com/lean-delivery/ansible-role-endeca-toolsandframeworks/master/LICENSE)

## Authors

[Lean Delivery team](team@lean-delivery.com)

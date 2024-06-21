# cisco-asa-backup

This Ansible script takes backup of multiple Cisco ASA devices and writes the results into output folder.

The inventory yaml format is simple as below.

```yml
---
all:
  children:
    all_cisco_asa:
      hosts:
        ciscoasa1:
          inventory_network_os: cisco.asa.asa
          inventory_network_cisco_asa_type: vpn
          inventory_network_cisco_asa_ha_type: standalone
          inventory_host: 192.168.1.98
          inventory_port: 22
          inventory_user: admin
          inventory_pass: password
    all_f5:
      hosts:
        bigip1:
          inventory_network_os: f5.bigip
          inventory_host: 192.168.1.245
          inventory_port: 443
          inventory_user: admin
          inventory_pass: password
        bigip2:
          inventory_network_os: f5.bigip
          inventory_host: 192.168.1.246
          inventory_port: 443
          inventory_user: admin
          inventory_pass: password
        bigip3:
          inventory_network_os: f5.bigip
          inventory_host: 192.168.1.247
          inventory_port: 443
          inventory_user: admin
          inventory_pass: password
```

## Prerequisite
Ansible ansible-pylibssh modules must be installed before running the script.
```bash
pip install ansible-pylibssh
```

## Usage:
```bash

ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook -vvvv --vault-password-file vault_pass.yaml -i inventory-vault.yaml cisco-asa-backup.yml --extra-vars="asa_device=ciscoasa1" --extra-vars="ha_type=standalone" --extra-vars="asa_type=vpn" --extra-vars="device_facts=true"
```

**_NOTE:_**  `output` folder must be created before running the script.


## Files
- cisco-asa-backup.yml >> Ansible script file
- vault_pass.yaml >> Inventory vault password information
- inventory-vault.yaml >> Inventory vault file encrypted by "Inventory vault password"

## Variables
- `asa_device` variable shows which Cisco ASA device/devices will be connected.The value can be "ciscoasa1", "ciscoasa2" or "v" as per the example inventory file above.
- `ha_type` variable is just a tag for definition of Cisco ASA device. It can be "active", "standby", "standalone" etc. If you assign "any" value, the script will take any type of "ha_type"
- `asa_type` variable is just another tag variable. You can define the firewall types as "vpn", "internal", "external" etc. If you assign "any" value, the script will take any type of "asa_type"
- `device_facts` variable shows the gathering facts paramater. If the value is true, then Device Facts will be retrieved and is written into output folder.

{
	"ansible_facts": {
		"ansible_network_resources": {},
		"ansible_net_gather_network_resources": [],
		"ansible_net_gather_subset": [
			"default",
			"hardware"
		],
		"ansible_net_system": "asa",
		"ansible_net_image": "boot:/asa9184-24-smp-k8.bin",
		"ansible_net_version": "9.18(4)24",
		"ansible_net_hostname": "ciscoasa",
		"ansible_net_device_mgr_version": "7.19(1)95",
		"ansible_net_api": "cliconf",
		"ansible_net_python_version": "3.10.12",
		"ansible_net_asatype": "ASA",
		"ansible_net_serialnum": "9A9841LNBBK",
		"ansible_net_filesystems": [
			"disk0:"
		],
		"ansible_net_filesystems_info": {
			"disk0:": {
				"spacetotal_kb": 8370192.0,
				"spacefree_kb": 8203096.0
			}
		},
		"ansible_net_memfree_mb": 713879,
		"ansible_net_memused_mb": 1302664,
		"ansible_net_memtotal_mb": 2016544
	},
	"failed": false,
	"changed": false
}

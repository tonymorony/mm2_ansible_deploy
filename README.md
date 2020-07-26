# Ansible template to deploy mm2 instance with activated DEX metrics

## General Pre-Requirements

- Ansible [installtion](https://docs.ansible.com/ansible/latest/installation_guide/index.html)

- SSH access to servers, mm2 software will be installed on.

Current version is meant to deploy on Ubuntu or Debian servers,
 you might need to modify tasks to deploy on different distribution.
Ports to be open on firewall:

 ```
7783/tcp -- mm2 rpc port
9000/tcp -- DEX_metrics public port
9100/tcp -- NodeExporter port
```

- Add your SSH key to shh-agent

```shell script 
$ eval `ssh-agent`
$ ssh-add /path/to/your/key
```

- Make sure your user@host combination is in known_hosts file,
 or disable ansible key checking `$ export ANSIBLE_HOST_KEY_CHECKING=False` (not recommended)

## MM2 deployment

- List your servers in `host.yml` file, example:

```yaml
...
    mm2instance00:
      hosts:
        "00.server.domain"
      vars:
        passphrase: "mm2passphrase0"
        ansible_user: ansibleuser
...
```
- Set desired mm2 version in `group_vars/all`

```yaml
...
mm2_tag: "beta-2.0.2179"
mm2_arch: "mm2-8c2bf3f91-Linux-Debug.zip"
mm2_link:
 "https://github.com/KomodoPlatform/atomicDEX-API/releases/download/{{ mm2_tag }}/{{ mm2_arch }}"
...
```

Use link to [official release](https://github.com/KomodoPlatform/atomicDEX-API/releases)

- You might want to customise your mm2 settings by editing `roles/mm2/templates/MM2.json.j2` file.
 Passprase (WIF) is set in `hosts.yml`,
 RPC_PASSWORD is in `group_vars/all`.

- From repo root execute:
 
 ```
 $ ansible-playbook deploy.yml -i hosts.yml
```

- Enjoy your mm2 installation
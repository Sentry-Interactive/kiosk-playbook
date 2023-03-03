# Kiosk Playbooks
Ansible playbooks for managing kiosks

## Testing
These testing instructions are aimed at development on an ARM64 based Mac, although the ansible playbook itself should 
work on any supported platform.

### Remote Testing
The playbooks can be tested against a remote VM or physical machine, ensure ansible is installed locally first.

```shell
ansible-galaxy install -r requirements.yml
ansible-playbook -i "192.168.202.181," local.yml
```

### Using Host Variables
Host variables (`host_vars`) allow overriding of certain properties for a specific host.

To view host variables for a specific host, use the following command:
```shell
ansible -i "MADBB9B05," all -m debug -a "var=hostvars.MADBB9B05"
```

Testing host variables locally can be done, but we may need to alter the way we do it to allow hostnames to be resolved, 
e.g. if our host is `MADBB9B05` but we can only contact it via an IP address or a mDNS hostname such as 
`MADBB9B05.local` we need to communicate this information to Ansible using a temporary inventory file.
```shell
TMPINV=`mktemp` && echo "MADBB9B05 ansible_host=MADBB9B05.local" > $TMPINV && ansible-playbook -i $TMPINV local.yml
```

### Install UTM & Setup Virtual Machine
UTM is a free virtualization tool based on QEMU, it is well suited to test our kiosk setup.

Alternatively, 
[VMware Fusion Tech Preview](https://customerconnect.vmware.com/downloads/get-download?downloadGroup=FUS-PUBTP-22H2) 
can achieve the same result, but you must use one of the daily Ubuntu ISO server images due to a bug.

1. Download UTM from https://mac.getutm.app/
2. Install UTM
3. Download Ubuntu 22.04 Jammy server edition from https://ubuntu.com/download/server/arm
4. Create a new virtual machine, ensure video acceleration is enabled

https://user-images.githubusercontent.com/6087327/199009533-9136f41b-fa56-4803-a989-d880683468cd.mov

### Configure Ubuntu 
To use the playbooks in this repository, we need to install git & ansible.

```shell
apt-get update && apt-get install -y git ansible-core
sudo ansible-pull -U https://github.com/Sentry-Interactive/kiosk-playbook.git -i "$(uname -n)," --diff
```

After these commands have completed, the UTM window should be displaying the kiosk.

## Ubuntu Auto Install Image
The ansible playbooks are designed to be trigger periodically to apply updates and at first boot - to enable first boot
operation, we customise an Ubuntu ISO using [Pxeless](https://github.com/cloudymax/pxeless). 

The following command will take the auto install data from `user-data-kiosk.yaml` and build an Ubuntu installer ISO 
image based on 22.04.

```shell
docker run \
  --platform linux/amd64 \
  --rm \
  --volume "$(pwd)/installer:/data" \
  --user $(id -u):$(id -g) \
  deserializeme/pxeless:latest \
  --use-release-iso \
  -a \
  -u user-data-kiosk.yaml \
  -n jammy
```

# TODO
- Add supervisor script/tool to ensure browser is running, if not restart gdm

# Credits
- https://github.com/jktr/ansible-pull-example
- https://help.gnome.org/admin/system-admin-guide/stable/lockdown-single-app-mode.html.en

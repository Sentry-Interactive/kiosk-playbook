# Kiosk Playbooks
Ansible playbooks for managing kiosks

## Testing
These testing instructions are aimed at development on an ARM64 based Mac, although the ansible playbook itself should 
work on any supported platform.

### Remote Testing
The playbooks can be tested against a remote VM or physical machine, ensure ansible is installed locally first.

```shell
ansible-galaxy install -r requirements.yml
ansible-playbook -i "192.168.202.181," local-lightdm.yml
```

### Install UTM & Setup Virtual Machine
UTM is a free virtualization tool based on QEMU, it is well suited to test our kiosk setup.

Alternatively, 
[VMware Fusion Tech Preview](https://customerconnect.vmware.com/downloads/get-download?downloadGroup=FUS-PUBTP-22H2) 
can achieve the same result, but you must use one of the daily Ubuntu ISO server images due to a bug.

1. Download UTM from https://mac.getutm.app/
2. Install UTM
3. Download Ubuntu 20.04 Jammy server edition from https://ubuntu.com/download/server/arm
4. Create a new virtual machine, ensure video acceleration is enabled

https://user-images.githubusercontent.com/6087327/199009533-9136f41b-fa56-4803-a989-d880683468cd.mov

### Configure Ubuntu 
To use the playbooks in this repository, we need to install git & ansible.

```shell
apt-get update
apt-get install -y git ansible-core
git clone https://github.com/mpbarnwell/ansible-test.git kiosk-playbook
cd kiosk-playbook
./run
```

After these commands have completed, the UTM window should be displaying the kiosk.



# Credits
- https://github.com/jktr/ansible-pull-example
# Kiosk Playbooks
Ansible playbooks for managing kiosks

## Testing
These testing instructions are aimed at development on an ARM64 based Mac, although the ansible playbook itself should 
work on any supported platform.

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

### Viewing Logs
Logs include console output from the web application running inside WebKit.

```shell
sudo snap log -n 100 wpe-webkit-mir-kiosk
```

### Changing Settings

#### Change the Kiosk URL
```shell
sudo snap set wpe-webkit-mir-kiosk "url=https://google.com"
```

#### cog.ini
cog.ini can be used to configure settings of the WebKit engine, it is typically located at 
`/root/snap/wpe-webkit-mir-kiosk/current/cog.ini` - documentation around these settings are not amazing, the following
links have some useful information/examples:

- https://github.com/Igalia/cog/pull/57
- https://webkitgtk.org/reference/webkit2gtk/stable/property.Settings.allow-file-access-from-file-urls.html

```cog.ini
[websettings]
enable-plugins = true
enable-media-stream = true
enable-mediasource = true
enable-webaudio = true
enable-webgl = true
user-agent = "Sentry Kiosk"
enable-media = true
enable-media-capabilities = true
set-permissions = all
enable_webrtc = true
```

When changing settings, you'll need to restart `wpe-webkit-mir-kiosk`

```shell
sudo snap restart wpe-webkit-mir-kiosk
```

# Ubuntu-frame & WebKit
Experimental UI approach based on WebKit and ubuntu-frame

## Viewing Logs
Logs include console output from the web application running inside WebKit.

```shell
sudo snap log -n 100 wpe-webkit-mir-kiosk
```

## Changing Settings

### Change the Kiosk URL
```shell
sudo snap set wpe-webkit-mir-kiosk "url=https://google.com"
```

### cog.ini
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
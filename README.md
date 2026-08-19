# U-Manager Files

A file browser for your Unraid server, installed as a Community Applications
plugin — a single Go binary with a bundled web UI, running on your server's
port `8740` and authenticated with your Unraid API key.

It powers the file browsing feature in the U-Manager mobile app, and it also
runs standalone in any browser at that same port.

## Table of Contents

- [Install the plugin](#install-the-plugin)
- [Setting it up in the U-Manager app](#setting-it-up-in-the-u-manager-app)
- [Signing in to the web UI](#signing-in-to-the-web-ui)
- [Reaching it from outside your network](#reaching-it-from-outside-your-network)
- [Server options](#server-options)
- [Hardware video transcode](#hardware-video-transcode)
- [Embedding in the Unraid webGUI](#embedding-in-the-unraid-webgui)
- [Update](#update)
- [Uninstall](#uninstall)
- [Support and bug reports](#support-and-bug-reports)
- [License](#license)

---

## Install the plugin

**From the U-Manager app** — open **CA Store** in the side menu, search for
*U-Manager Files* and tap **Install**. You need to be connected to the server
already, since the app installs it through the Unraid API.

**From the Unraid WebGUI** — open **Plugins → Install Plugin** and paste:

```
https://github.com/jandrop/u-manager-files-releases/releases/latest/download/UManagerFiles.plg
```

Click **INSTALL**.

Either way, the plugin starts on port `8740` with `/mnt/user` as the exposed
root.

## Setting it up in the U-Manager app

Open the **File Browser settings** in the app (you need to be connected to a
server first).

**Connection**

- **Host** / **Port** — your Unraid server and port `8740`. The plugin runs on
  Unraid itself, so the host is simply your server's address.
- **HTTPS** — turn on if you reach the server over `https`.

**Authentication**

- **API key** — paste an Unraid API key. Recommended: create a **new,
  dedicated** key in Unraid (**Settings → Management Access → API Keys →
  Create API Key**) so you can later revoke just the file browser's access
  without touching your other keys.

**Preferences**

- **Native file browser** — browse with the app's built-in UI instead of the
  bundled web UI.
- **Hardware transcoding** and **Media thumbnails** — server-side options you
  toggle from the app. They need a recent plugin version; older versions show
  them disabled with an "update needed" hint. Showing previews in the native
  file browser requires the **File Browser Pro** add-on.

**Advanced**

- **Custom headers** — extra HTTP headers sent with every request, for servers
  behind a reverse proxy (e.g. Cloudflare Access).

Use **Test connection** to confirm the app can reach the service, then **Save**.

## Signing in to the web UI

The login screen asks for an API key. Any key issued by Unraid itself is
accepted, so you sign in with a key Unraid already gave you. Besides the
WebGUI route above, you can create one from the server terminal:

```
unraid-api apikey --create
```

Paste the key it prints into the login screen. The session lives in a cookie,
so you only do this once per browser.

The **Static API key** setting is separate and optional. It adds one fixed key
of your own for scripts or clients that cannot use an Unraid key.

## Reaching it from outside your network

Keep port `8740` on your LAN. Do not forward it on your router, and do not
publish it through a reverse proxy. This is a file browser with read and write
access to your whole array, so a door opened to it is a door opened to your
data, and servers exposed to the internet get found and probed automatically.

Use a VPN instead. Unraid ships WireGuard built in, under **Settings → VPN
Manager**. It takes a few minutes to set up, and once your phone is connected
it reaches the plugin exactly as if it were sitting on your LAN, with nothing
published to the internet. Unraid's own guide covers the reasoning:

https://unraid.net/blog/unraid-server-security-best-practices

## Server options

In Unraid, go to **Settings → User Utilities → U-Manager Files**:

- **Root directory exposed (`BASE_PATH`)** — `/mnt/user` by default, which
  shows your shares. Use `/mnt` to also reach unassigned disks (USB drives and
  anything else outside the array).
- **Listen port** — `8740` by default. Pick another if that port is taken. The
  service listens on every interface; to bind it to one address instead, set
  `ADDR` by hand in `/boot/config/plugins/umfiles/umfiles.env`, for example
  `ADDR=127.0.0.1:8740`, then run `/usr/local/sbin/rc.umfiles restart`.
- **Static API key (optional)** — empty means Unraid sign-in only. Set a value
  to also accept that key in an `X-API-Key` header.
- **Allow embedding in a frame (`FRAME_ANCESTORS`)** — empty by default, which
  blocks every site from framing the UI. See
  [Embedding in the Unraid webGUI](#embedding-in-the-unraid-webgui).
- **Hardware video transcode (Intel iGPU)** (`ENABLE_TRANSCODE`) — off by
  default. See [Hardware video transcode](#hardware-video-transcode).
- **Image and video thumbnails** (`ENABLE_THUMBNAILS`) — on by default. Turn
  it off if browsing large media folders feels slow; the cache stays on disk,
  but new thumbnails are no longer rendered.

**Apply** restarts the service automatically.

The same page shows the size of the thumbnail cache and a **Purge thumbnails**
button that wipes it. Thumbnails are rebuilt on demand the next time each file
is shown.

## Hardware video transcode

Off by default. Turn it on and videos the browser cannot decode are transcoded
on the fly through VA-API, so HEVC, AV1 and 4K play anywhere.

It needs an Intel GPU and the **Intel GPU TOP** plugin from Community
Applications. Image and video thumbnails work without either.

**Transcode GPU** picks the card. Leave it on Auto unless the server has more
than one Intel GPU. Available only on plugin versions newer than 2026.07.29.1.

## Embedding in the Unraid webGUI

Available only on plugin versions newer than 2026.07.28.1.

By default no site can put U-Manager Files inside a frame, which is what
blocks clickjacking. To reach it from a tab in the webGUI (with the Custom Tab
plugin, for example), list the origin you are embedding from in **Allow
embedding in a frame**, scheme included:

```
http://192.168.1.10
```

Use commas for several. Note that the webGUI and U-Manager Files run on
different ports, and the port is part of the origin, so a same-origin policy
is not enough. Put the webGUI address there, not the port 8740 one.

## Update

The plugin auto-checks for new versions through `releases/latest`. You can
also force a check from **Plugins → CHECK FOR UPDATES**.

## Uninstall

**Plugins → U-Manager Files → Remove**.

## Support and bug reports

Open an issue on the plugin repository:

https://github.com/jandrop/u-manager-files-releases/issues

## License

MIT. See
[LICENSE](https://github.com/jandrop/u-manager-files-releases/blob/main/LICENSE).

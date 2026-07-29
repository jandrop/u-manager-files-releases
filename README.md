# U-Manager Files

Native Unraid file browser as a Community Apps plugin. Single Go binary
with a bundled web UI, authenticated against your Unraid API key.

Powers the U-Manager mobile app and runs in any browser at the Unraid
host's port 8740.

## Install

In the Unraid WebGUI, open **Plugins** -> **Install Plugin** and paste:

```
https://github.com/jandrop/u-manager-files-releases/releases/latest/download/UManagerFiles.plg
```

Click **INSTALL**. The plugin starts the service on port 8740 with
`/mnt/user` as the exposed root.

## Authentication

The login screen always asks for an API key. Any key issued by Unraid itself is accepted, so you sign
in with a key Unraid already gave you.

To create one, run this on the server terminal:

```
unraid-api apikey --create
```

Paste the key it prints into the login screen. The session lives in a
cookie, so you only do this once per browser.

The **Static API key** setting is separate and optional. It adds one
fixed key of your own for scripts or clients that cannot use an Unraid
key.

## Configuration

Go to **Settings** -> **Other Settings** -> **U-Manager Files** to change:

- **Root directory exposed (`BASE_PATH`)**: `/mnt/user` by default. Use
  `/mnt` to also reach unassigned disks (USB drives, etc.).
- **Listen address (`ADDR`)**: `:8740` by default. Set
  `127.0.0.1:8740` to bind to localhost only, or pick another port if
  8740 is taken.
- **Static API key**: optional. Adds a fixed key of your own on top of
  the keys Unraid issues. See [Authentication](#authentication).
- **Allow embedding in a frame (`FRAME_ANCESTORS`)**: empty by default,
  which blocks every site from framing the UI. See
  [Embedding in the Unraid webGUI](#embedding-in-the-unraid-webgui).

Apply restarts the service automatically.

## Embedding in the Unraid webGUI

Available only on plugin versions newer than 2026.07.28.1.

By default no site can put U-Manager Files inside a frame, which is what
blocks clickjacking. To reach it from a tab in the webGUI (with the
Custom Tab plugin, for example), list the origin you are embedding from
in **Allow embedding in a frame**, scheme included:

```
http://192.168.1.10
```

Use commas for several. Note that the webGUI and U-Manager Files run on
different ports, and the port is part of the origin, so a same-origin
policy is not enough. Put the webGUI address there, not the port 8740
one.

## Update

The plugin auto-checks for new versions through `releases/latest`.
You can also force a check from **Plugins** -> **CHECK FOR UPDATES**.

## Uninstall

**Plugins** -> **U-Manager Files** -> **Remove**.

## Support and bug reports

Open an issue on this repo:

https://github.com/jandrop/u-manager-files-releases/issues

## License

MIT. See [LICENSE](LICENSE).

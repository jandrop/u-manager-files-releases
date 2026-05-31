# U-Manager Browser

Native Unraid file browser as a Community Apps plugin. Single Go binary
with a bundled web UI, authenticated against your Unraid API key.

Powers the U-Manager mobile app and runs in any browser at the Unraid
host's port 8740.

This repo is the public release mirror. The source code lives in a
private repo. Each tagged release publishes a `.plg` and a versioned
tarball here.

## Install

In the Unraid WebGUI, open **Plugins** -> **Install Plugin** and paste:

```
https://github.com/jandrop/u-manager-browser-releases/releases/latest/download/UManagerBrowser.plg
```

Click **INSTALL**. The plugin starts the service on port 8740 and seeds
a configuration file at `/boot/config/plugins/umbrowser/umbrowser.env`.

Edit that file (set `BASE_PATH`, `API_KEY` if you want a static key,
etc.) and restart with `/usr/local/sbin/rc.umbrowser restart`.

## Update

The plugin auto-checks for new versions through `releases/latest`.
You can also force a check from **Plugins** -> **CHECK FOR UPDATES**.

## Uninstall

**Plugins** -> **U-Manager Browser** -> **Remove**.

## Support and bug reports

Open an issue on this repo:

https://github.com/jandrop/u-manager-browser-releases/issues

## License

MIT. See [LICENSE](LICENSE).

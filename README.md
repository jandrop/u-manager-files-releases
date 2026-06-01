# U-Manager Files

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
https://github.com/jandrop/u-manager-files-releases/releases/latest/download/UManagerFiles.plg
```

Click **INSTALL**. The plugin starts the service on port 8740 with
`/mnt/user` as the exposed root.

## Configuration

Go to **Settings** -> **Other Settings** -> **U-Manager Files** to change:

- **Root directory exposed (`BASE_PATH`)**: `/mnt/user` by default. Use
  `/mnt` to also reach unassigned disks (USB drives, etc.).
- **Listen address (`ADDR`)**: `:8740` by default. Set
  `127.0.0.1:8740` to bind to localhost only, or pick another port if
  8740 is taken.
- **Static API key**: optional. Leave empty to use Unraid single sign-on
  only. Set a value to also accept it in the `X-API-Key` header (useful
  for scripts).

Apply restarts the service automatically.

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

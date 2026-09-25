# HyDE Settings

A searchable settings hub for [HyDE](https://github.com/HyDE-Project/HyDE): one
place to find HyDE's own tools and the system settings programs already on a
HyDE install (Audio, Displays, Network, Appearance, Accounts, Firewall, ...),
instead of having to remember which separate app each one lives in.

Most entries resolve to tools HyDE already ships or recommends (Pavucontrol,
nwg-displays, nm-connection-editor, nwg-look, font-manager, Flatseal,
`kcmshell6` for KDE KCMs, ...) and just give them one searchable front door,
rather than reimplementing each one's UI. A few things are built in directly:
a read-only account overview, a copyable system-information page, and a
weather-location search for Waybar's weather module.

It is a single GTK 3 window: categories on the left, the tools of the selected
category on the right, and a global search (Ctrl+F) across all of them. It takes
its colours from the current Wallbash/Waybar theme and follows theme switches
live. Entries whose tool is not installed stay visible with the package that
provides it; nothing is installed automatically.

## Installation

This repository is a HyDE *extra dot*: `HyDE-Project/HyDE` pulls a tagged
release of it through `deez` (see `Scripts/dots/settings.toml` there). Select
it with the other extra dots during `install.sh`, or deploy it on its own from
a HyDE checkout:

```sh
deez --config Scripts/dots/settings.toml dots --deploy hyde-settings
```

The dot installs `settings.py` and `settings.sh` into `~/.local/lib/hyde/` and
three launchers into `~/.local/share/applications/`. It is not meant to be run
standalone outside a HyDE install: it depends on `hyde-shell`, HyDE's Wallbash
colours and other HyDE state.

Dependencies (declared by the dot, Arch package names): `python-gobject`, `gtk3`.
Python 3.11 or later is required. The optional tools behind individual entries
are listed in the [manual](docs/manual.md#available-areas-and-tools).

## Usage

Open **HyDE Settings** from the application launcher or Waybar's HyDE menu, or:

```sh
hyde-shell settings                 # open the window (a second call focuses it)
hyde-shell settings --system-info   # print the system overview as JSON, no GTK needed
hyde-shell settings --help
```

Keyboard shortcuts, the theme contract and failure behaviour are described in
[`docs/manual.md`](docs/manual.md).

## Development

```sh
sh settings.sh                          # preview this checkout against your installed HyDE
python3 tests/settings_test.py          # logic checks
python3 tests/settings_test.py --gtk    # + GTK integration checks (needs Xvfb)
```

Requires Python 3.11+, GTK 3 and PyGObject; the GTK checks also need Xvfb and
xauth. [`docs/tests.md`](docs/tests.md) holds the acceptance matrix behind the
suite, including what CI does not cover.

HyDE pins a tagged release, not `main`, so a change only reaches users after a
new tag is created and HyDE's `settings.toml` is updated to it.

## License

GPLv3, same as HyDE -- see [LICENSE](LICENSE).

# lxqt-kwin-session

>Files for an LXQt wayland session using kwin_wayland.

For the working panel this is based on [wayland-taskbar PR](https://github.com/lxqt/lxqt-panel/pull/2031).
Make sure `/usr/share/applications/lxqt-panel.deskop`  matches installation path.
For desktop, notifications, runner and session Qt6-port PRs are needed.

Use your display manager to start "LXQt Kwin (Wayland)".
`startlxqtkwin` from tty should work too.

## Screenshots

![LXQt-kwin dark](lxqt-kwin.png)

LXQt style "Dark"; Palette "Valendas"

## Dependencies

Build dependencies are `CMake`, [lxqt] 2.0>= and optionally
`Git` to pull latest VCS checkouts. [kwin] version 6.0.2 or higher is recommended.

### Optional:

* `systemsettings` for configuring kwin
* `plasma-workspace` for configuring shortcuts and else


## Installation

Code configuration is handled by CMake.<br>
CMake variable `CMAKE_INSTALL_PREFIX` has to be set to `/usr` on most operating systems.

```
git clone https://github.com/stefonarch/lxqt-kwin-session.git
cd lxqt-kwin-session
mkdir build && cd build
cmake ..  -DCMAKE_INSTALL_PREFIX=/usr  -DCMAKE_BUILD_TYPE=Debug && make -j4

# Prefer creating a package for your distro instead of using sudo make install

```

## Packages:

For Arch based distributions an AUR package will be available.

## Known Issues and Notes

* `kwin_wayland` sets scaling to 1.25 by default if not configured under plasma, see
`~/.config/kwinoutputconfig.json` value `"scale": 1,`.

* LXQt lock settings do not work yet. Plasma/kwin  screenlocker works. To disable
screenlock add `  --no-lockscreen` to the options in `startlxqtkwin`.

* Bottom and right panel have some alignment issues with gaps and alignment to screen
border.

* Button settings in `lxqt-powermanagement` do not apply yet.
  
* Some X11-only applications (example: redshift) in autostart
  could lead to high CPU usage under wayland.

* Scale and language settings are imported from LXQt. (TODO keyboard)
  
* Mouse cursor and size are synced and can be set using "Appearance" settings,
session restart required. GTK settings have to be updated after changes. TODO

* Global shortcuts are handled by kwin in `~/.config/kglobalshortcutsrc`.



[AUR]:                    https://aur.archlinux.org/packages/lxqt-kwin-session-git

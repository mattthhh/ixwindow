# Ixwindow – iconized xwindow 

## About
Ixwindow is an enhanced version of standard `xwindow` polybar module. The main
feature is that `ixwindow` displays not only info about active window, but also 
an icon for it. It also allows you more customization of printing window info.
Below is represented an example of what `ixwindow` looks like in action:

### Bspwm

[bspwm_demo.webm](https://user-images.githubusercontent.com/32775308/223551520-a605d964-29f6-4602-874d-2b1b254d432c.webm)

### I3

[i3_demo.webm](https://user-images.githubusercontent.com/32775308/223551794-b1300ef6-f65a-4d05-89d3-9af3be25765f.webm)


**Note:** basically, it doesn't really depend on polybar itself, it can be used 
with any other bar as well, you just need to implement the same behavior,
as polybar's `tail = true`.


## Dependencies

### Common 
- [`cargo`](https://github.com/rust-lang/cargo)
For cargo installation instructions, see [here](https://github.com/rust-lang/cargo).

### For bspwm
- `bspwm`

### For i3
- `i3`


## Downloading

If you want to install stable version, then you should download the source code 
from the `master` branch using the following command:
```bash
git clone git@github.com:andreykaere/ixwindow.git && cd ixwindow
```
If you want the bleeding edge version, switch to `dev` branch.

### Installation

To install it to your system you just have to run `./install` . 
To see installation options run `./install --help`. 
You will also need to add the following to your polybar `config` file:
```dosini
[module/ixwindow]
type = custom/script
exec = /path/to/ixwindow
tail = true
```
and put it somewhere on bar, for example, add it right next to your window
manager: `modules-left = <wm> ixwindow`. 

If you have multi monitors setup, then you need to have distinct bar for
each of them and create modules like this:
```dosini
[module/ixwindow1]
type = custom/script
exec = /path/to/ixwindow <name_of_monitor_1>
tail = true

[module/ixwindow2]
type = custom/script
exec = /path/to/ixwindow <name_of_monitor_2>
tail = true
...
```
and then put these modules on respective bars:
```dosini
...
; Bar for monitor1
[bar/bar1]
...
modules-left = <wm> ixwindow1
...

; Bar for monitor2
[bar/bar2]
...
modules-left = <wm> ixwindow2
...

...
```

## Configuration

Default configuration file is supposed to be located at
`$XDG_CONFIG_HOME/ixwindow/ixwindow.toml`. If you want it to be located
somewhere else, you should specify that in environmental variable
`IXWINDOW_CONFIG_PATH`, or run `ixwindow` script with
`--config=<path_to_config>` option.

In config file, there are various options, that can be modified (example of
configuration file can be found in `examples/ixwindow.toml`), such as:
- background color of polybar bar
- size of icon
- coordinates for icon
- path to folder for cached icons (note: it makes sense to keep it 
around `.config/polybar` folder, so you won't lose your custom icons, 
if you have them)
- `gap` constant, which is used in `ixwindow` script 
- for `i3` you can additionally specify `gap_per_desk`. This variable is used
  for calculation position of the icon, when the number of active desktops is
  dynamic.

To change your configuration, just edit your config file. For new settings to
take affect, you have to restart polybar (for example with `polybar-msg cmd
restart`).

## Generating icons

`ixwindow` uses the output of `xprop` for generating icons automatically. 
Most of the times it works, but for some applications, for example, Spotify,
it doesn't. In this case, if you want to have an icon for these applications,
you have to add them manually. 

### Adding custom icons

Sometimes it's not possible to get icon using `xprop`, (for example, it's the 
case with Spotify and Discord), then you have to add them manually to your 
`polybar-icons` folder. To do that, you need to have `png` or `svg` version
of the icon, named as `WM_CLASS` (which you can find by running `xprop
WM_CLASS` and selecting your app). Then you run the following command
(requires `imagemagick`): 
```bash
ixwindow-convert --size <size> --color <color> --cache <chache_dir> <icon-name>
```
where `<icon-name>` is the right name as described above. This will convert
icon to `jpg` format with appropriate background color and move to your cache 
directory. 

**Note:** This method can be also used for replacing automatically generated
icons. 

**Note:** Basically all apps have icons on your system in `png` or `svg`
format. Usually, one can find it somewhere in `/usr/share/icons` directory
(one can use `find` or `fd` utility for it).

You can try it out on some icons located in `examples/custom-icons` folder.

## Known issues & limitations

- Lack of png support, but replacing `jpg` with `png` would require compositor 
as the dependency as well
- Manual specification, but seems to be unfixable at this point, since polybar 
doesn't support inserting images into bar for now

Feel free to open issue if you have any questions or you've noticed a bug.
Also pull requests are welcome; don't hesitate to crate one, if you have a
solution to any of the issues, stated above.

## Goals

- Add png support (maybe make it an option, if user doesn't use compositor)

## Thanks

### Inspired by

I got inspired to start this project, when I saw similar feature in
`awesome-wm`. I thought it would be hard to simulate the exact same behavior
on polybar, however I came across
[this](https://github.com/MateoNitro550/xxxwindowPolybarModule) project and I
thought, that I can just improve the default `xwindow`, by formatting its
output a bit and adding icon of the focused application.

### With a great help of

I would like to thank  [psychon](https://github.com/psychon) for helping me 
understand `x11rb` and xorg in general, which made it possible to rewrite code
in rust and drastically improve that project.


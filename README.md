# s2arrows

Interception plugin for mapping `s`+`hjkl` keys to arrow keys.

<a href="http://www.catonmat.net/blog/why-vim-uses-hjkl-as-arrow-keys/">
    <img src="http://www.catonmat.net/images/why-vim-uses-hjkl/adm-3a-hjkl-keyboard.jpg" alt="ADM-3A terminal">
</a>

## What is it?

This is a plugin to migrate the second most useful feature of Karabiner for
MacOSX to Linux. With this plugin enabled, `s` acts as a modifier key. While
`s` is pressed, the `h` `j` `k` `l` keys act as arrow keys, following the VI
key bindings.

It also makes the `s` + `f` combination as a different modifier that maps the
`h`, `j`, `k` and `l` keys to `Home`, `Page Down`, `Page Up` and `End`
respectively. This is very convenient for laptops since you will not be needing
the fn combination anymore. And in general, it means your right hand will have
to move even less often.

## Why?

It's so nice in VIM that you don't have to lift your right hand to move to the
arrow keys. It would be great if you could do the same in all applications. In
fact, once you get used to this, it's very hard to go back.

## Dependencies

- [Interception Tools][interception-tools]

Apart from This I have also added my keybingings of dual [function](dual-function-keys/my-mappings.yaml) keys as well.

## Building

```sh
git clone git@github.com:abhinav3398/s2arrows
cd s2arrows
mkdir build
cd build
cmake ..
make
sudo make install
```

## Enabling

### udevmon

`s2arrows` is an [_Interception Tools_][interception-tools] plugin. A suggested
`udevmon` job configuration (`/etc/udevmon.yaml`) is:

```yaml
- JOB: "intercept -g $DEVNODE | s2arrows | uinput -d $DEVNODE"
  DEVICE:
    EVENTS:
      EV_KEY: [KEY_S, KEY_H, KEY_J, KEY_K, KEY_K, KEY_LEFT, KEY_DOWN,
               KEY_UP, KEY_RIGHT]
```

If you already have an [_Interception Tools_][interception-tools] plugin
running, like, I don't know, [caps2esc][caps2esc], you should pipe the two
commands to each other and append the keys to the list instead:

```yaml
- JOB: "intercept -g $DEVNODE | caps2esc | s2arrows | uinput -d $DEVNODE"
  DEVICE:
    EVENTS:
      EV_KEY: [KEY_CAPSLOCK, KEY_ESC, KEY_S, KEY_H, KEY_J, KEY_K, KEY_K,
               KEY_LEFT, KEY_DOWN, KEY_UP, KEY_RIGHT]
```

Also, here is my setup with [dual-function-keys](https://gitlab.com/interception/linux/plugins/dual-function-keys) plugin:

```yaml
- JOB: "intercept -g $DEVNODE | s2arrows | dual-function-keys -c /etc/interception/dual-function-keys/my-mappings.yaml | uinput -d $DEVNODE"
  DEVICE:
    NAME: "AT Translated Set 2 keyboard"
    EVENTS:
      EV_KEY: [KEY_CAPSLOCK, KEY_LEFTALT, KEY_ESC, KEY_H, KEY_J, KEY_K, KEY_L,
              KEY_LEFT, KEY_DOWN, KEY_UP, KEY_RIGHT, KEY_U, KEY_D, KEY_G, KEY_LEFTSHIFT]
```

### systemd

Then, you can enable and start the service like any other:

```sh
sudo systemctl enable udevmon.service
sudo systemctl start udevmon.service
```

## Caveats

The main problem with using this plugin is that the `s` key might be
interpreted as a modifier, when in fact you actually want to press `s`. This
can happen when you type something like `ssh some_host` very fast. The `sh`
part from the sentence might be interpreted as a press of the left arrow key
and the end result may appear as ` some_hosts`. The same problem used to happen
with Karabiner too. Over time, you will get used to letting an extra
millisecond pass after every `s` key press.

Another caveat is that with this plugin enabled, you lose the ability to hold
the `s` key down and have the character repeat itself. From my experience, this
has never been a problem for me.

## License

<a href="https://gitlab.com/interception/linux/plugins/caps2esc/blob/master/LICENSE.md">
    <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/0/0b/License_icon-mit-2.svg/120px-License_icon-mit-2.svg.png" alt="MIT">
</a>

[caps2esc]: https://gitlab.com/interception/linux/plugins/caps2esc
[interception-tools]: https://gitlab.com/interception/linux/tools


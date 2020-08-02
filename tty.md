# tty

`Ctrl+Alt+F3` will bring up the login prompt of tty3.

If you log in and issue the `tty` command, you’ll see you are connected to `/dev/tty3`.

This isn’t a **pseudo-teletype** (emulated in **software**); it is a **virtual teletype** (emulated in **hardware**). It is using the screen and keyboard connected to your computer, to emulate a virtual teletype like the DEC VT100 used to do.

You can use function keys `Ctrl+Alt` with function keys `F3` to `F6` and have four TTY sessions open if you choose. For example, you could be logged into tty3 and press `Ctrl+Alt+F6` to go to tty6.

To get back to your graphical desktop environment, press `Ctrl+Alt+F2`.

Pressing `Ctrl+Alt+F1` will return you to the login prompt of your graphical desktop session.

At one time, `Ctrl+Alt+F1` through to `Ctrl+Alt+F6` would open up the full-screen TTY consoles, and `Ctrl+Alt+F7` would return you to your graphical desktop environment. If you are running an older Linux distribution, this might be how your system behaves.

This was tested on current releases of Manjaro, Ubuntu, and Fedora and they all behaved like this:

- `Ctrl+Alt+F1`: Returns you to the graphical desktop environment log in screen.
- `Ctrl+Alt+F2`: Returns you to the graphical desktop environment.
- `Ctrl+Alt+F3`: Opens TTY 3.
- `Ctrl+Alt+F4`: Opens TTY 4.
- `Ctrl+Alt+F5`: Opens TTY 5.
- `Ctrl+Alt+F6`: Opens TTY 6.

https://www.howtogeek.com/428174/what-is-a-tty-on-linux-and-how-to-use-the-tty-command/#:~:text=Pressing%20Ctrl%2BAlt%2BF1%20will,to%20your%20graphical%20desktop%20environment.

# GNU Screen

`Screen` allows you to start shells or programs in a virtual terminal which you may then disconnect and reconnect from a different location. For example, you can start a shell at work; disconnect; go home; and reconnect to the same shell. A single screen session can group multiple shells and you can have multiple screen sessions.

Documentation warning: the documentation for GNU Screen is inconsistent; the documentation uses `C-x` notation to indicate a `CTRL-X` key combination, but all the screen config files and run-time messages use `^x` notation to indicate a `CTRL-X`.

## Commandline Options

Although `screen` is a GNU project, it does not follow the GNU convention for CLI options where you use -- in front of a long option. So '-ls' does not indicate multiple options; although, for other options it might. It's best not to worry about it and just remember the few basic options that you commonly need.

* `screen` -- with no options this starts new screen session with a shell in a virtual screen. From inside the session you type `C-a C-d` to disconnect the session.
* `screen -r` -- reconnect to a disconnected session.
* `screen -rd` -- disconnect a session the is connected somewhere else and then reconnect. Use this if you change terminals and left a session connected elsewhere.
* `screen -ls` -- list all sessions (connected and disconnected).


`screen /dev/xx.usbserial-XXXXXXXX 115200 –L`

## Control Commands (used inside of a screen session)

All screen commands start with `C-a`. Then most of the commands follow with a key that you may type either with `CTRL` or without. For example, the disconnect command may be typed as either `C-a`, `C-d` or as `C-a d`.

* `C-a ?` -- Show help
* `C-a d` -- Disconnect the session. The session continues in the background as a daemon which you can reconnect to later.
* `C-a K` -- KILL the current screen
* `C-a c` -- Create a shell in new virtual screen. This screen is added to the current session.
* `C-a n` -- Next screen
* `C-a p` -- Previous screen
* `C-a esc` -- Enter Copy/Scrollback mode (esc again to exit scrollback mode). Scrollback mode is very useful.
    * Use Vi-like keys to move around the scrollback history (the usual `h`,`j`,`k`,`l` for cursor, `C-u` for half page UP, `C-d` for half page DOWN).
    * Press `SPACE` to start selecting a region. Press `SPACE` again to Yank the region.
    * Press `C-a ]` to paste the yanked text into the current screen.

## Using screen as an `RS-232`/serial terminal
Screen makes a very good serial port terminal. You can connect a screen window to any serial device in `/dev`. Most serial devices in linux are named like `/dev/ttyS0`, `/dev/ttyAMA0`, and `/dev/serial0` for a built-in serial port; or for USB-to-serial adapters the names are usually like `/dev/ttyUSB0` and `/dev/ttyUSB1`.

The communication settings are a comma separated list of control modes as would be passed to `stty`. See the man page for `stty` for more info.

Old, slow-speed serial devices usually play nice with `9600 8N1` (9600 baud, 8-bits per character, no parity, and 1 stop bit):

```bash
screen /dev/ttyS0 9600,cs8,-parenb,-cstopb,-hupcl
screen /dev/ttyS0 19200,cs8,-parenb,-cstopb,-hupcl
screen /dev/ttyS0 115200,cs8,-parenb,-cstopb,-hupcl
# use odd parity:
screen /dev/ttyS0 9600,cs8,parenb,parodd,-cstopb,-hupcl
# even parity:
screen /dev/ttyS0 9600,cs8,parenb,-parodd,-cstopb,-hupcl
```
## Connecting to the Nucleo Board

For the STM32 Nucleo board you can find the device under the `/dev/` folder. There are always 2 devices associated with the board. You can quickly find them by using:

```bash
ls /dev/{cu,tty}.usbmodem*
```

You can then connect to the serial port using:

```bash
screen /dev/tty.usbmodem1234 9600,cs8,-parenb,-cstopb,-hupcl
```

### References
These notes have been taken (and edited) from the following sources:
* [Noah.org GNU Screen Notes](http://www.noah.org/wiki/Screen_notes#using_screen_as_a_serial_terminal)
* [Setting up a Serial Terminal with Mac OS X](https://software.intel.com/en-us/setting-up-serial-terminal-on-system-with-mac-os-x)
* [MAC OS Driver (Virtual COM Port) for STM32 USB](https://community.st.com/s/question/0D50X00009XkfDHSAZ/mac-os-driver-virtual-com-port-for-stm32-usb)
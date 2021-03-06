Run in GUI (run-in-gui)
============================

| Donate to support this free software |
|:------------------------------------:|
| <img width="164" height="164" title="" alt="" src="doc/bitcoin.png" /> |
| [1DmVQkuY1zjP6ZiPi1QJgSowHtR8afBDMS](bitcoin:1DmVQkuY1zjP6ZiPi1QJgSowHtR8afBDMS) |

This project contains two programs that assist you in executing programs
in a separate desktop session (defined by the environment variables of any
program executing in that session), and then switching to the session in
real-time as the program executes.  It's useful to start GUI programs from
cron that need a graphical display and/or audio access.

Why is this necessary?
----------------------

Lots of people ask me why this is useful, or some other variant of the same
question, such as "Can't I just `DISPLAY=:0` in my crontab?"

Well, because of the way that Linux has evolved, nowadays it's very hard to
get certain computer programs to do things that were trivial in the past.
Lots of GUI programs now depend on a wide array of session daemons and
other services, such as:

* X11 authentication,
* D-Bus,
* KWallet,
* GnuPG agents,
* SSH agents,
* KIO services,
* PulseAudio.

These services are normally set up properly at the start of (and solely
in the context of) your GUI session, but entirely unavailable for use in
crontabs and other non-GUI shells, nominally disassociated with your
desktop session.

For example: getting your music player to play a list of songs to wake you
up -- unless you're running the program straight from a terminal in your
desktop session's GUI, it won't actually work; either the program just
won't run, or the audio will be corked.

How this works
--------------

So what we do in `run-in-gui` is simple: ''we cheat'' by stealing the
necessary information from other programs running in your desktop session,
and using it to recreate an environment in which we run your program.

Caveat: of course, this program ''still'' requires you to be logged in. If
your session isn't active, `run-in-gui` will take the liberty to switch to
your inactive session, activating -- but, if locked, not unlocking -- it,
and then running your program.

Tools included in this set
--------------------------

This package contains several tools:
    
1. `run-in-env-of`: a tool that runs the command of your choice in the
    environment of the PID of your choice.  If run as root, it can
   `setuid()` to the user and group of that PID.
2. `run-in-gui`: a tool that sorts out a desktop session you're running
   then uses run-in-env-of to execute the command on your choice under
   that desktop session.  Prior to the execution of that program, it
   switches to the selected desktop session, to enable hardware access
   to devices that would otherwise be blocked (e.g. audio).

What you need to have before using this package
-----------------------------------------------
    
* Python 2.7 on your local machine
* `loginctl` fully operational
* `/proc` mounted

Installation
------------

You will need to install this package on your local machine.

You can install this package directly from PyPI using pip::

    pip install run-in-gui

If you are on an RPM-based distribution, build an RPM from the source package
and install the resulting RPM::
    
    python setup.py bdist_rpm

Otherwise, just use the standard Python installation system::

    python setup.py install

You can also run it directly from the unpacked source directory::
    
    export PYTHONPATH="$PWD"/src
    ./<program> <parameters>

Usage
-----

Say, for example, you want to run Amarok from a crontab, but you want Amarok
to start on your logged-in desktop session.  No problem: you can stick this
into your crontab:

    20 4 * * * /usr/local/run-in-gui/bin/run-in-gui amarok -p Toke\ up.m3u

As you can see, at exactly 4:20 AM, Amarok will be executed in your desktop
session, and will start playing the playlist `Toke up.m3u` in your home
directory.

You can also do the exact same thing from an `at` job:

    echo /path/to/run-in-gui amarok -p Matanga.m3u | at 5:45

If, for some reason, the program won't start, check your mail folder in
`/var/mail/$USERNAME`, where cron traditionally deposits crontab output.

Run in GUI (run-in-gui)
============================

| Donate to support this free software |
|:------------------------------------:|
| <img width="164" height="164" title="" alt="" src="doc/bitcoin.png" /> |
| [12cXnYY3wabbPmLpS7ZgACh4mBawrXdndk](bitcoin:12cXnYY3wabbPmLpS7ZgACh4mBawrXdndk) |

This project contains two programs that assist you in executing programs
in a separate desktop session (defined by the environment variables of any
program executing in that session).  It's useful to start GUI programs from
cron.

Tools included in this set
--------------------------

This package contains several tools:
    
    1. run-in-env-of: a tool that runs the command of your choice in the
       environment of the PID of your choice.  If run as root, it can
       setuid() to the user and group of that PID.
    2. run-in-gui: a tool that sorts out a desktop session you're running
       then uses run-in-env-of to execute the command on your choice under
       that desktop session.

What you need to have before using this package
-----------------------------------------------
    
    - Python 2.7 on your local machine
    - `loginctl` fully operational
    - `/proc` mounted

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

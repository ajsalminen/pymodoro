# Pymodoro

This is a modified version of Pymodoro with working, but very rough implementation of the following features:
commands to stop the timer, log interruptions during the pomodoro, ability to pass a name for the pomodoro being worked on and
basic logging of work that has been done.

## Running

To run, put the files into a new folder called **.pymodoro** inside your home folder. Configure your xmobar to display pymodoro with

    Run CommandReader "~/.pymodoro/pymodoro.py" "pomodoro"

Then add it to your template.

## Install

To install Pymodoro system wide, run the setup.py script like this:

    python setup.py install

## Usage

A new Pomodoro -- 25 minutes followed by a break of 5 minutes -- is started by writing a start command to ~/.pomodoro_session.
An example of how to do that with php is

    php -r 'print json_encode(array("timestamp"=>time(), "command" => "start", "taskname" => "mytask"))' > ~/.pomodoro_session

You can use that or something similar. Other commands are stop (to stop the timer) and interrupt (for logging an
interruption that happened during the pomodoro). Create a script you can call from xmonad, your todo list manager
etc. For example I use something like this to start the timer in emacs

    (defun pymodoro-start ()
      "Write a command to start pymodoro for the todo.txt item
    on the current line"
      (interactive)
      (let (todoItem)
          (setq todoItem (substring (thing-at-point 'line) 0 -1))
            (setq pymodoroCommand (json-encode `(:command start :timestamp ,(float-time) :taskname ,todoItem)))
            (write-region pymodoroCommand nil log-file nil)))



### Keybindings

The easiest way is to define keybindings for the commands.

#### Xmonad

Configure xmonad to start a new Pomodoro session by adding a call to your timer start script to your xmonad.hs:

    -- start a pomodoro (replace scriptname with your own)
    , ((modMask, xK_n), spawn "/path/to/scriptname")

Or:

    -- start a pomodoro
    , ("M-n", spawn "touch /path/to/scriptname")

This way, whenever you hit modMask + n, you will start a new pomodoro.

## Configure

You can set all the options via command line paramters. For a detailed description run:

    ~/.pymodoro.py --help

It is no longer needed to edit the script itself. If you still want to do it, open up the file **~/.pymodoro/pymodoro.py**.

## Credits

* Thanks to Mirko Horstmann for [the ticking sound](http://www.freesound.org/people/m1rk0/sounds/50070/).
* Session and Break sounds from Soothing Alerts by [Adam Dachis](http://adachis.kinja.com)

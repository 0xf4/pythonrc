PYTHONRC
========
Initialization script for the interactive Python interpreter. Its main purpose
is to enhance the overall user experience when working in such an environment
by applying some improvements to the standard console.

It also works with IPython and BPython, although its utility in that kind of
scenarios can be argued.

Tested in GNU/Linux with Python versions 2.7 and 3.4.

Please read the Installation section below.


Features
--------
- User input completion
    + Introduces a completion mechanism for inputted commands in Python 2.
    + In Python 3, where the standard console is a lot nicer, it just
    impersonates the default completion machinery to keep the consistency with
    the behavior in Python 2 (and so it's still possible to adapt it to the
    user's needs).

- Command History
    + Creates a callable, singleton object called `history`, placing it into
    the `__builtins__` object to make it easily available, which enables the
    handling of the command history (saving some input lines to a file of your
    choice, listing the commands introduced so far, etc.). Try simply
    `history()` on the Python prompt to see it in action; inspect its members
    (with `dir(history)` or `help(history.write)`) for more information.

- Color prompt
    + Puts a colorful prompt in place, if the terminal supports it.


Installation
------------
- You must define in your environment (in GNU/Linux and MacOS X that usually
means your `~/.bashrc` file) the variable 'PYTHONSTARTUP' containing the path
to `pythonrc.py`.

- It is also highly recommended to define the variable 'PYTHON_HISTORY_FILE'.
Remember that BPython (unlike the standard interpreter or IPython) ignores that
variable, so you'll have to configure it as well by other means to be able to
use the same history file there (for instance, in Linux, the file
`~/.config/bpython/config` is a good place to start, but please read BPython's
documentation).

### Example configurations
- Extract of `~/.bashrc`
```sh
# python
export PYTHONSTARTUP=~/.python/pythonrc.py
export PYTHON_HISTORY_FILE=~/.python/.python_history

## You may want to also uncomment some of this lines if using virtualenvwrapper
# export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3.4
# export WORKON_HOME=~/.python/virtualenvs
# source $(which virtualenvwrapper.sh)
```

- Extract of `~/.config/bpython/config`
```
[general]
color_scheme = default
hist_file = ~/.python/.python_history
hist_lenght = 1000
```

Bugs / Caveats / Future enhancements
------------------------------------
- No module/package introspection for the last argument in commands of the form
`from <package> import <not_completing_this>` (this, in fact, could be a not so
bad thing, because it doesn't execute side effects, e.g. modules' init code).

- Depending on the user's system, the compilation of the packages' and modules'
list for completing `import ...` and `from ... import ...` commands can take a
long time, especially the first time it is invoked.

- When completing things like a method's name, the default is to also include
the closing parenthesis along with the opening one, but the cursor is placed
after it no matter what, instead of between them. This is because of the
python module `readline`'s limitations.
You can turn off the inclusion of the closing parenthesis; if you do so, you
might be also interested in modifying the variable called
`dict_keywords_postfix` (especially the strings that act as that dictionary's
indexes).

- IPython has its own `%history` magic. I did my best to not interfere with
it, but I don't know the actual consequences. Also, it's debatable if it
even makes sense to use this file with IPython and/or BPython (though having
a unified history for all the environments is really nice).
You could define some bash aliases like
```sh
alias ipython='PYTHONSTARTUP="" ipython'
alias bpython='PYTHONSTARTUP="" bpython'
```
to be on the safer side.

- Could have used the module `six` for better clarity. Right now it uses my own
made up stubs to work on both Python 2 and 3.

- Needs better comments and documentation, especially the part on history
handling.

- Probably a lot more. Feel free to file bug reports ;-)

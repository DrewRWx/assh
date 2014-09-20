assh
========

Purged of all bashisms and able to be run anywhere.

* Set your shell.
* Set your home directory.
* Set your variables and aliases.

Usage
-----

`assh user@host.com` - Logs in.

`assh -A user@host.com` - Takes SSH flags.

`assh user@host.com "ls"` - Runs commands.

* _assh_ is best used in conjunction with _ssh_config_: `assh -A user@host.com` -> `assh name`.

* _assh_ constructs an _ssh_ call by parsing its `.asshrc` file. This is most of mine:

~~~
# SAMPLE .asshrc CONFIGURATION FILE FOR assh

# Any lines before a "Host" line is declared are included by default.
# If a variable is defined multiple times, the last redefinition will be used.

# These are used in conjunction with SSH Agent to push changes from a remote machine.
export GIT_AUTHOR_NAME="Drew Waranis";
export GIT_AUTHOR_EMAIL=drew@waran.is;
export GIT_COMMITTER_NAME="Drew Waranis";
export GIT_COMMITTER_EMAIL=drew@waran.is;

# I chose the Green Party of console text editors.
# Meanwhile, my mentor chose emacs as a sane default.
export EDITOR="nano -xc";
export GIT_EDITOR="nano -xc";
export VISUAL="nano -xc";

# The primary motivation behind this project was to set aliases.
# Stupid, petty aliases... that were mostly factored out by appending the PATH.
alias nano="nano -xc";
alias ls="ls --color=auto -lah";
alias grep="grep --color";
alias ps="ps aux";
alias fps="ps aux | grep ";
alias apt-upgrade="sudo apt-get update && sudo apt-get dist-upgrade";

# Selected lines from .asshrc are executed in a shell, so commands can be run.
# For example, this restores ctrl-u to deleting text to the left of the cursor in zsh.
bindkey \^U backward-kill-line;

# Hostnames on the "Host" line are matched by regex.
# For example, the first entry matches 'machine' followed by two numbers.
Host machine[0-1][0-9] old03 virtual
	export PATH=/nas/drew/local/bin:$PATH;

	# Executing parts of .asshrc also allow for scripting.
	# The shared machines I use default to bash and uses a lot of aliases, hence:
	if [ -e "${HOME}/.bash_aliases" ]; then
		source "${HOME}/.bash_aliases";
	fi

	# HomeSelect sets a custom HOME directory to start in.
	HomeSelect /nas/drew

	# By default, your current shell is selected for use.
	# ShellSelect overrides this on a host by host basis.
	ShellSelect zsh
	export PATH=~/local/bin:$PATH;

~~~

* _assh_ looks for `.asshrc` in:

  1. `$HOME/.ssh/.asshrc`

  2. `$HOME/.asshrc`

  3. `$HOME/.config/.asshrc`

  4. `./.asshrc`

and picks the first one it finds.

* If `.asshrc` is not found, shell forwarding will still function.

Installation
------------

Drop it into your PATH .

Tested on:

* OS X Maverick's _bash_ in _sh_ compatibility mode.
* Ubuntu 13.04 an 14.04's _dash_.

License
------------

* FreeBSD 2-clause license.

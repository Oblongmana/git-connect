git + hub + lab + other connections = git-connect
==================

[hub](https://github.com/github/hub) is a command line tool that wraps `git` in 
order to extend it with extra features and commands that make working with 
GitHub easier.

`git-connect` is a fork of [github/hub](https://github.com/github/hub) that 
[@Oblongmana](https://github.com/Oblongmana) uses, and maybe some of the folks 
at [@Trineo](https://github.com/Trineo) might as well maybe. 

This is likely going to be a bit of a hodge-podge of different functionality,
but the aim is to get you as connected with the outside world as possible from
within your command line. The less often you have to leave and open a browser
or app, the better. Less clicking, more typing.

Enhancements over regular git + hub:
 - gitlab integration (WIP - available in the 
    [gitlab](https://github.com/oblongmana/git-connect/tree/gitlab)) branch
 - github `issues` verb: `hub issues [more-specific-verb]`, aiming to extend to
    include gitlab issues as well. Includes the following `more-specific-verb`s:
   - list (with default open-only one-line listing, and verbose and close issues 
      flags)
   - new (with flags for setting title, body, assignee, and labels)
   - close (a specific issue number)
   - labels (lists all the labels on a repo)
 - [MavensMate](https://github.com/joeferraro/MavensMate) flag to launch the 
    `new_project_from_existing_directory` dialog after cloning a project, to add
    MavensMate nature to the project
   - `hub clone [-s|--salesforce]`
 - new `init` flag `[-s|--salesforce]`. Does regular `init` stuff, plus:
   - `curl`s a [standard Salesforce dev .gitignore from a gist]
      (https://gist.github.com/Oblongmana/7130387/raw/.gitignore-sf) designed
      for use with Sublime Text and MavensMate on OSX
   - creates a `README.md`
 - `nuke` verb: following the procedure on GitHub's [Remove Sensitive Data]
     (https://help.github.com/articles/remove-sensitive-data) page, expunges a
     file from your local history - completely writing it out and making it 
     appear it was never there. Instructions for pushing to remotes are then 
     output to terminal
 

This is particularly oriented towards [@ForceDotCom]
(https://github.com/ForceDotCom) development using [Sublime Text]
(http://www.sublimetext.com/) (especially ST3), with the 
[kemayo/sublime-text-git](https://github.com/kemayo/sublime-text-git/) plugin, 
and the [joeferraro/MavensMate](https://github.com/joeferraro/MavensMate) 
server + plugin, running on OSX - as that's my daily working environment. In the event 
that this repo goes somewhere, that may not be ideal - some of that 
functionality should probably be left out of master, or perhaps have separate
subdirectories for different focuses and corresponding installation flags,
or perhaps forked projects. I like the sound of subdirectories. But that's not
a concern for now, so none of that complexity for now. Feel free to open an 
issue if that changes in future.

All that said, none of the aforementioned things are required to install,
but of course their corresponding functionality won't work (e.g. the -s flag on
clone won't work if you don't have MavensMate. Should just be doing `git`-ish 
things, nothing else. If it is doing something else, that's probably a 
bug. Except maybe the OSX requirement. Should hopefully work ok on linux, but
I'm not touching Windows with a barge-pole. If core `hub` works on Windows,
you're probably ok though. Maybe.

I'll do my best to keep this up to speed with the main `hub` repo, so this 
provides enhancement without holding you back.

This can't be installed alongside core `hub` - as it is in fact `hub` + stuff

`hub` is best aliased as `git`, so you can type `$ git <command>` in the shell and
get all the usual `hub` features. See "Aliasing" below.


Installation
------------

Dependencies:

* **git 1.7.3** or newer
* **Ruby 1.8.6** or newer [Probably, only recently tested on 1.9.3. Open an issue if any problems]

### `rake install` from source

Unlike the core `hub`, this can only be installed through rake install from source.
This can't be installed alongside core `hub`. 

~~~ sh
# Clone the project from GitHub:
$ git clone git@github.com:Oblongmana/git-connect.git
$ cd git-connect
$ rake install
~~~

On a Unix-based OS, this installs under `PREFIX`, which is `/usr/local` by default.

Now you should be ready to roll:

~~~ sh
$ hub version
git version 1.7.6
hub version 1.8.3
~~~

### Help! It's slow!

Check out the section on slowness in the core [hub](https://github.com/github/hub)


Aliasing
--------

Using hub feels best when it's aliased as `git`. This is not dangerous; your
_normal git commands will all work_. hub merely adds some sugar.

`hub alias` displays instructions for the current shell. With the `-s` flag, it
outputs a script suitable for `eval`.

You should place this command in your `.bash_profile` or other startup script:

~~~ sh
eval "$(hub alias -s)"
~~~

### Shell tab-completion

hub repository contains tab-completion scripts for bash and zsh. These scripts
complement existing completion scripts that ship with git.

Disclaimer: I haven't touched this in `git-connect` - it's somewhere on a todo 
list (and may not necessarily need messing with)

* [hub bash completion](https://github.com/github/hub/blob/master/etc/hub.bash_completion.sh)
* [hub zsh completion](https://github.com/github/hub/blob/master/etc/hub.zsh_completion)


Use with [joeferraro/MavensMate](https://github.com/joeferraro/MavensMate) and [kemayo/sublime-text-git](https://github.com/kemayo/sublime-text-git/) in Sublime Text
--------
Open your User Settings for the [kemayo/sublime-text-git](https://github.com/kemayo/sublime-text-git/) package, and add the following (NOTE that your hub install location may vary, run `which hub` to check the location):

~~~ json
{
    "git_command": "/usr/local/bin/hub"
}
~~~

As [kemayo/sublime-text-git](https://github.com/kemayo/sublime-text-git/) invokes the git program directly, this simply tells it that the 
git program it should be invoking is our hub program.

In future, I aim to fork [kemayo/sublime-text-git](https://github.com/kemayo/sublime-text-git/) and add some of the `hub` specific features


Commands
--------

Disclaimer: I haven't touched this section of the README much in `git-connect` - 
I found there were a few things missing from this list that were features in 
core `hub`, so caveat emptor. The source isn't super crazy to read (check out
[lib/hub/commands.rb](lib/hub/commands.rb), and the methods have comments up
indicating usage where standard git has been extended. This is on a todo to look
at eventually, maybe. Have added to [git init](#git-init) below, check that out

Assuming you've aliased hub as `git`, the following commands now have
superpowers:

### git clone

    $ git clone schacon/ticgit
    > git clone git://github.com/schacon/ticgit.git

    $ git clone -p schacon/ticgit
    > git clone git@github.com:schacon/ticgit.git

    $ git clone resque
    > git clone git@github.com/YOUR_USER/resque.git

### git remote add

    $ git remote add rtomayko
    > git remote add rtomayko git://github.com/rtomayko/CURRENT_REPO.git

    $ git remote add -p rtomayko
    > git remote add rtomayko git@github.com:rtomayko/CURRENT_REPO.git

    $ git remote add origin
    > git remote add origin git://github.com/YOUR_USER/CURRENT_REPO.git

### git fetch

    $ git fetch mislav
    > git remote add mislav git://github.com/mislav/REPO.git
    > git fetch mislav

    $ git fetch mislav,xoebus
    > git remote add mislav ...
    > git remote add xoebus ...
    > git fetch --multiple mislav xoebus

### git cherry-pick

    $ git cherry-pick http://github.com/mislav/REPO/commit/SHA
    > git remote add -f mislav git://github.com/mislav/REPO.git
    > git cherry-pick SHA

    $ git cherry-pick mislav@SHA
    > git remote add -f mislav git://github.com/mislav/CURRENT_REPO.git
    > git cherry-pick SHA

    $ git cherry-pick mislav@SHA
    > git fetch mislav
    > git cherry-pick SHA

### git am, git apply

    $ git am https://github.com/defunkt/hub/pull/55
    [ downloads patch via API ]
    > git am /tmp/55.patch

    $ git am --ignore-whitespace https://github.com/davidbalbert/hub/commit/fdb9921
    [ downloads patch via API ]
    > git am --ignore-whitespace /tmp/fdb9921.patch

    $ git apply https://gist.github.com/8da7fb575debd88c54cf
    [ downloads patch via API ]
    > git apply /tmp/gist-8da7fb575debd88c54cf.txt

### git fork

    $ git fork
    [ repo forked on GitHub ]
    > git remote add -f YOUR_USER git@github.com:YOUR_USER/CURRENT_REPO.git

### git pull-request

    # while on a topic branch called "feature":
    $ git pull-request
    [ opens text editor to edit title & body for the request ]
    [ opened pull request on GitHub for "YOUR_USER:feature" ]

    # explicit title, pull base & head:
    $ git pull-request -m "Implemented feature X" -b defunkt:master -h mislav:feature

### git checkout

    $ git checkout https://github.com/defunkt/hub/pull/73
    > git remote add -f -t feature mislav git://github.com/mislav/hub.git
    > git checkout --track -B mislav-feature mislav/feature

    $ git checkout https://github.com/defunkt/hub/pull/73 custom-branch-name

### git merge

    $ git merge https://github.com/defunkt/hub/pull/73
    > git fetch git://github.com/mislav/hub.git +refs/heads/feature:refs/remotes/mislav/feature
    > git merge mislav/feature --no-ff -m 'Merge pull request #73 from mislav/feature...'

### git create

    $ git create
    [ repo created on GitHub ]
    > git remote add origin git@github.com:YOUR_USER/CURRENT_REPO.git

    # with description:
    $ git create -d 'It shall be mine, all mine!'

    $ git create recipes
    [ repo created on GitHub ]
    > git remote add origin git@github.com:YOUR_USER/recipes.git

    $ git create sinatra/recipes
    [ repo created in GitHub organization ]
    > git remote add origin git@github.com:sinatra/recipes.git

### git init

    $ git init -g
    > git init
    > git remote add origin git@github.com:YOUR_USER/REPO.git

    $ hub init [-s | --salesforce]
    > git init
    > curl -#o .gitignore https://gist.github.com/Oblongmana/7130387/raw/.gitignore-sf
    > touch README.md

### git push

    $ git push origin,staging,qa bert_timeout
    > git push origin bert_timeout
    > git push staging bert_timeout
    > git push qa bert_timeout

### git browse

    $ git browse
    > open https://github.com/YOUR_USER/CURRENT_REPO

    $ git browse -- commit/SHA
    > open https://github.com/YOUR_USER/CURRENT_REPO/commit/SHA

    $ git browse -- issues
    > open https://github.com/YOUR_USER/CURRENT_REPO/issues

    $ git browse schacon/ticgit
    > open https://github.com/schacon/ticgit

    $ git browse schacon/ticgit commit/SHA
    > open https://github.com/schacon/ticgit/commit/SHA

    $ git browse resque
    > open https://github.com/YOUR_USER/resque

    $ git browse resque network
    > open https://github.com/YOUR_USER/resque/network

### git compare

    $ git compare refactor
    > open https://github.com/CURRENT_REPO/compare/refactor

    $ git compare 1.0..1.1
    > open https://github.com/CURRENT_REPO/compare/1.0...1.1

    $ git compare -u fix
    > (https://github.com/CURRENT_REPO/compare/fix)

    $ git compare other-user patch
    > open https://github.com/other-user/REPO/compare/patch

### git submodule

    $ git submodule add wycats/bundler vendor/bundler
    > git submodule add git://github.com/wycats/bundler.git vendor/bundler

    $ git submodule add -p wycats/bundler vendor/bundler
    > git submodule add git@github.com:wycats/bundler.git vendor/bundler

    $ git submodule add -b ryppl --name pip ryppl/pip vendor/pip
    > git submodule add -b ryppl --name pip git://github.com/ryppl/pip.git vendor/pip

### git ci-status

    $ git ci-status [commit]
    > (prints CI state of commit and exits with appropriate code)
    > One of: success (0), error (1), failure (1), pending (2), no status (3)


### git help

    $ git help
    > (improved git help)
    $ git help hub
    > (hub man page)


Configuration
-------------

### GitHub OAuth authentication

Hub will prompt for GitHub username & password the first time it needs to access
the API and exchange it for an OAuth token, which it saves in "~/.config/hub".

### HTTPS instead of git protocol

If you prefer using the HTTPS protocol for GitHub repositories instead of the git
protocol for read and ssh for write, you can set "hub.protocol" to "https".

~~~ sh
# default behavior
$ git clone defunkt/repl
< git clone >

# opt into HTTPS:
$ git config --global hub.protocol https
$ git clone defunkt/repl
< https clone >
~~~


Contributing
------------

These instructions assume that _you already have hub installed_ and aliased as
`git` (see "Aliasing").

1. Clone hub:  
    `git clone oblongmana/git-connect && cd git-connect`
1. Ensure Bundler is installed:  
    `which bundle || gem install bundler`
1. Install development dependencies:  
    `bundle install`
2. Verify that existing tests pass:  
    `bundle exec rake`
3. Create a topic branch:  
    `git checkout -b feature`
4. **Make your changes.** (It helps a lot if you write tests first.)
5. Verify that tests still pass:  
    `bundle exec rake`
6. Fork hub on GitHub (adds a remote named "YOUR_USER"):  
    `git fork`
7. Push to your fork:  
    `git push -u YOUR_USER feature`
8. Open a pull request describing your changes:  
    `git pull-request`


Meta
----

* Home: <https://github.com/github/hub>
* Bugs: <https://github.com/github/hub/issues>
* Gem: <https://rubygems.org/gems/hub>
* Authors: <https://github.com/github/hub/contributors>

### Prior art

These projects also aim to either improve git or make interacting with
GitHub simpler:

* [hub](https://github.com/github/hub)
* [eg](http://www.gnome.org/~newren/eg/)
* [github-gem](https://github.com/defunkt/github-gem)


# PyGit
============

Automate the boring git stuff with python

## Motivation

Whenever I wanted to see the status of all my git repos I have to fire up the
`git-cmd.exe` shell on windows, navigate to each folder and then do a `git status`.
I have to do this both at home and at work.

But I got quickly tired of it. So I decided to make this tool to give me a quick
report so I can see what is ahead and what's behind and what's ahead at a glance.
In short, what needs attention so as to avoid those troubling merge conflicts.

## Requirements

The only requirements in terms of software is `send2trash` which helps take care of cleaning up stuff.
Other thing you need is a computer with `git` accessible from the command line. If you're on a computer
where you don't have permission to install stuff, you can do with a portable `git` and `pygit` will work just fine.

You can get a portable git version from [here](https://git-scm.com/download/win)

Just unzip it and place it somewhere on your disk. You'll need to tell `pygit` where this file is during intitialization.


## Installation

Github, `pip install https://github.com/immensity/pygit/archive/3.0.tar.gz` or PyPI `pip install pygit --upgrade`

## Usage

Upon successful installation do

   import pygit

A successful import returns a blank screen.

To use `pygit`, you have to tell it where your `git` repositories are. You may do this by passing it a list of strings on the command line.
Each string represents a full path name to single directory.

If you have a master directory that holds multiple `git` repositories, `pygit` can also take the full path name of this master directory
and then index the git repositories it finds there. It won't index those directories that are not git repos.

It is also possible to tell `pygit` not to index certain directories by specifying the starting string of the directory name. This is referred
to s a `rule`. Directories matching such rules will not be touched.




To initialize `pygit`, run

   pygit.initialize()

In case things change (perhaps you moved folders around) and you want to reset your folders,
just run the `set_all()` command again


To see all available repositories run ::

   pygit.show_repos()

This command shows a list of all available repositories in the format ::

   repository_id: repository_name: repository_directory_path

To load a particular repository use ::

   pygit.load(repo_id_or_name)

where **repo_id** is a string-valued id assigned to that particular repo. The first value in the `show_repos` command's output.


The **load\(\)** command returns a `Commands` object for that repo, which provides a gateway for issuing git commands on the repository

Operations that can be performed on `Commands` object are shown below. ::

   Commands().fetch() # perform fetch.

   Commands().status() # see status

   Commands().add_all() # stage all changes for commit

   Commands().commit(message='minor changes') # commit changes. Press enter to accept default message

   Commands().push() # perform push action

   Commands().pull() # perform pull request

   Commands().add_commit() # add and commit at once



### Batch Operations

Pygit provides some functions for performing batch operations on your repositories. ::

   pygit.load_multiple(*args)

loads a set of repositories. You could decide to only load only 2 of 10 repositories. Perhaps you need to perform similar actions
on those two alone. As an example

   pygit.load_set("2", "5")

returns a  `generator`  of  `Commands`  objects for repositories 2 and 5. Afterwards you can use a  :py:func:`for`  loop to iterate over the repos
like below

   for each in pygit.load_set("2", "5"):
      each.add_commit()

   pygit.all_status()


performs a **status** command on all your repositories. The result is written to a text file. The text file opens automatically.
The name of the file shows the date and time of the status request. All batch status request is written to its a separate file.
Call it a snapshot of your repo status if you will
Those items which are out of sync with their remote counterpart (by being ahead or being behind) are also highlighted as needing attention. ::

   pygit.pull_all()


performs a **pull** request on all your repositories at once. Its  `return`  value is  None. ::

   pygit.push_all()


performs a **push** action on all your repositories at once. Its  `return` value is  None. ::

   pygit.load_all()


returns a  `generator`  of  `Commands`  object for every repository.


API
=====
.. automodule :: pygit.api
   :members:

Commands
=============

.. automodule :: pygit.commands
   :members:

Utility Functions
=======================

.. automodule :: pygit.utils
   :members:

To do
======

Add **git-bash.exe**

Implement Commands.branch()

Find out why importing pygit for first time gives an PermissionError
Write tests

Run test after importation to make sure every other thing works fine.

Define an update function that updates the repo dictionaries for the case when a new repo is added but the overall directory structure remains unchanged.
Copyright (c) 2012-2014 Max Oberberger (max@oberbergers.de)

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the 
GNU General Public License for more details.

You should have received a copy of the GNU General Public License 
along with this program.  If not, see <http://www.gnu.org/licenses/>.

* * *


Table of Contents
=================
1. [Introduction]()
2. [System Requirements]()
3. [Installation]()
4. [Using git-jira-hook]()
5. [Credits]()
6. [References]()
7. [Notes]()


1. Introduction
===============
The JIRA SOAP API ist not developed any more. It will be deprecated beginning
with JIRA 5.x. Therefore I switched my git-hook from SOAP to REST. If someone
wants to see the SOAP-hook, just checkout the soap git tag (git checkout soap).

1.1 Get git and Jira to work in Harmony
---------------------------------------
If you are using git [1] for source control and Jira [2] for bug 
tracking, then the git-jira-hook script might be useful to you.

Once you have the script setup in your environment, every time you 
make a git commit, a comment is automatically posted to an open 
issue in Jira.

This script will also "enforce" that for every git commit, you have at
least one Jira issue that you are referencing.

This way you have a paper trail of the history behind each and every
git commit.

This is particularly useful for corporate git repositories.


1.2 Example use
---------------
In order to specify which issue (or issues) you want the commit message 
to get tracked to in Jira, you place magic text markers such as:

 "NNN"

anywhere in your commit message, where "NNN" is the name of an open 
Jira issue.

For example, say you have typed in the following commit message in
your git repository.

    Hey look! This is my first git-commit

    Using the new and fresh git-jira-hook

    SW-189, HW-278 


The following things will happen:

In your Jira bug database, for projects named "TST", "HW" and "FW"
in Jira, for issue numbers "SW-189", "HW-278", and "FW-702", the 
following comments will be added:

    commit 424daa955f5c8a17aab9d524071f65f1999769a9
    Author: Max Oberberger <max@oberbergers.de>
    Date: Tue Aug 21 17:12:39 2012 +0200

    Hey look! This is my first git-commit

    Using the new and fresh git-jira-hook

    SW-189, HW-278

1.3 Wait! there's  more
-----------------------

If your git repository is exposed using gitweb [3], an hyperlink
linking to the exact commit will also be embedded in the Jira issue
comment that was added. This enables anyone to examine your git
commit simply by clicking on the hyperlink.

2. System Requirements
======================
- Python 2.x and python modules: jira-python (available at Bitbucket:
  https://bitbucket.org/bspeakmon/jira-python)
  I have tested with Python 2.7.4.
- A Jira installation with Remote APIs enabled.
- git version 1.6.x.y (I have tested with 1.8.1.2).
- Linux or some other similar Unix flavor (I have tested with 
  Ubuntu 12.03 LTS).
- OPTIONAL, but Highly recommended: gitweb [4] which has been 
  setup with "upstream" git repositories.

3. Installation
===============
Here is a typical example of how this hook may be used.

In a corporate setting where git is used, there is typically an 
"upstream" or "public" repository. And then developers have their 
"private" repositories.  For their day-to-day work, the developers 
use their private repositories.  Periodically, they (either 
directly, or via gatekeepers) push changes from their private 
repository to the "upstream" one. Also, the "upstream" repository 
is typically a bare repository, and no actual commits are done
here.

The hook is completely installed in the "upstream" repository. In my case I have
a special "commit"-User. With this User it is easier to filter "commit"-emails
from jira. Therefore you must insert a username and a password of the
"commit"-User in the hook.

Whenever a commit is made in the upstream repository or a "git push" 
is done to it, the  installed hook will kick in and validate the 
commit message, followed by update of the Jira issue.


3.1 Installation for "upstream" repository
------------------------------------------
- Copy this script to 
  upstream-repo-GIT-dir/hooks/{pre-receive|update|post-receive} 
  and mark it executable

      Example:
       chmod +x git-jira-hook
       cp git-jira-hook upstream-project.git/hooks/pre-receive
       cp git-jira-hook upstream-project.git/hooks/update
       cp git-jira-hook upstream-project.git/hooks/post-receive

See the "Frequently Asked Questions" section to figure out what values 
to use for your "jira.url" and "gitweb.url"


4. Using git-jira-hook
======================
When you are read to make a git commit, make sure that you have an 
appropriate open jira issue. There can be more than one open issues.
Lets say this commit deals with Jira issues FOO-23 and BAR-42  and 
also marks FOO-56 as resolved.

Anywhere in your commit message, you must put the following strings 
(without the quotes):
   "FOO-23"
   "BAR-42"


And then, at a later time, when you do "git push" to push your changes
upstream, the final validation and Jira issue update will be done.


5. Credits
==========
This script was inspired by the following:

- http://github.com/dreiss/git-jira-attacher/tree/master
- https://github.com/chiemseesurfer/git-trac-hook
- http://jira-python.readthedocs.org/en/latest/


6. References
=============
[1] Git, an source configuratiin management ("SCM") tool
    http://git-scm.com/

[2] Jira,  a bug tracking system
    http://www.atlassian.com/software/jira

[3] Gitweb, a web based browser for git
    http://git.or.cz/gitwiki/Gitweb
    

7. Notes 
=============
- JIRA REST at bitbucket: https://bitbucket.org/bspeakmon/jira-python


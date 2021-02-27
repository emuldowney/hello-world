---
layout:     post
title:      Python venv is not fully encapsulated
date:       2021-02-27
summary:    Virtual environments still depend upon the original installation
categories: programming
tags: programming
image: "/images/venv-problems/python39-install.png"
---

# Python venv doesn't work across multiple user accounts on the same PC

Recently I came across an unusual issue with python virtual environments and how they are implemented on Windows. 
Before I begin, some notes about this post.
 - This issue arose in work, and I have replicated it on my personal laptop for the purposes of this post. 
 - What I am doing with the virtual environments may not be conventional, so buyer-beware.
 - I have tested that this issue occurs on python 3.6-3.9 and can also be resolved by the method shown.

Take a shared PC, with multiple Windows user accounts. I wished to create a shared python project that all users can access on the PC. We have some test software that many people use on a small number of shared PCs. The setup process is slightly involved, whereas the software must be used by operators less familiar with python. Ideally, I would set-up once, and allow multiple users to run the shared project. Let me take you through the process.

#### Setting up my local PC environment
Using the windows installer, [Python 3.8 installs](https://docs.python.org/3/using/windows.html#installation-steps) in the following directory:
```
C:\Users\<username>\AppData\Local\Programs\Python\Python38
```
I create a new project on the C:\ drive. And install a virtual environment to isolate it from all the other projects on the pc. The directory structure looks something like:
```
C:\test_project\
|
| venv\
| test.py
```

I can now activate the virtual environment and run the python project.
```
venv\Scripts\activate
python3 test.py
```
The output is shown in the image:
<div class="showcase" align="center">
  <img style="width:75%" src="/images/venv-problems/venv-pass.png" alt="Python venv working correctly">
    <p class="meta">Python venv working correctly</p>
</div>

Notice the installation directories of python on the PC, given by the command `where python3`.
Note how the project was created on the `C:\` drive. This project is now accessible to other users of the PC.

#### Issues arise when another user accesses the project

Along comes another user, and they log-in with their user account. Now they try to access this shared project on the `C:\` drive; This is what they are presented with:
<div class="showcase" align="center">
  <img style="width:75%" src="/images/venv-problems/venv-fail.png" alt="Python venv not working on another user account">
    <p class="meta">Python venv not working on another user account</p>
</div>

So even with all the promises of the venv that my python installation is fully isolated, and contained in the `venv\` directory, there is something wrong. Lets investigate further.

- Venv still depends upon the default python installation, even after copying the binaries
- The default python installation is at a User Account level, not system level
- These combine to break the venv when logged in under a different user account.

Nowhere in the [documentation](https://docs.python.org/3/library/venv.html) for venv does it suggest that this would be a problem. There are no stack overflow questions with this problem. It took a bit of a deep-dive to figure out the problem.

##### Into the pyenv rabbit-hole...

The first hint is found in this quote from the [venv documentation](https://docs.python.org/3/library/venv.html):

>When a virtual environment is active (i.e., the virtual environment’s Python interpreter is running), the attributes sys.prefix and sys.exec_prefix point to the base directory of the virtual environment, whereas sys.base_prefix and sys.base_exec_prefix point to the non-virtual environment Python installation which was used to create the virtual environment.

When a python virtual environment is active, it knows where it is being run from (makes sense), and also knows the directory of its source installation (why??). (I never found out why, I didn't need to after I found a workaround)

What else is in that `venv\` directory?
```
venv\
|
| Scripts\
| Lib\
| Include\
| pip-selfcheck.json
| pyvenv.cfg
```
What is in that config file?
```
home = C:\Users\mi-one\AppData\Local\Programs\Python\Python36
include-system-site-packages = false
version = 3.6.3
```
There is a reference to the system installation of python!

So most of my google searches have come up fruitless. Time to move onto the bug reporting. A site search of [bugs.python.org](http://bugs.python.org) turns up the following relevant links:
 - [Make pyvenv style virtual environments easier to configure when embedding Python](https://bugs.python.org/issue22213)
 - [Make it easier to use a venv with an embedded Python interpreter](https://bugs.python.org/issue35706)
 - [python3 -m venv NAME`: virtualenv is not portable](https://bugs.python.org/issue36964)
 - [Support for relative home path in pyvenv.cfg](https://bugs.python.org/issue39469)
 - [venv on Windows with symlinks is broken if invoked with -I](https://bugs.python.org/issue42013)
 - [Add path expansion interpolation in pyvenv.cfg home key](https://bugs.python.org/issue34507)

The links fall into two categories:
 - Embedding python interpreter into another program/project
 - Making venv folders portable

Now, my use case is not exactly either of the above, but making venv folders portable is very similar to my desired use case. Reading the bugs, the impression that I get is that the maintainers have no interest in making virtual environments portable. That is their own prerogative. What can I do to make this work for my use case?

### Solution: Change the installation path
Let me try to install python again.

This time I will change the installation directory, and install directly on the `C:\` drive.
<div class="showcase" align="center">
  <img style="width:75%" src="/images/venv-problems/python39-install.png" alt="A fresh install of Python 3.9.2">
    <p class="meta">Install python on the C:\ drive</p>
</div>

I quickly set up a virtual environment in the same project directory, and test that all is working on my user account.
<div class="showcase" align="center">
  <img style="width:75%" src="/images/venv-problems/venv-pass-py39.png" alt="A fresh install of Python 3.9.2">
    <p class="meta">Install python on the C:\ drive</p>
</div>

And testing the installation on another user account:
<div class="showcase" align="center">
  <img style="width:75%" src="/images/venv-problems/venv-py39-test-user.png" alt="A fresh install of Python 3.9.2">
    <p class="meta">Install python on the C:\ drive</p>
</div>

Success!! This works because the new installation directory `C:\python39` is accessible from all user accounts on the PC.

Now we can run the project from multiple user accounts on the same PC, all from the same virtual environment.
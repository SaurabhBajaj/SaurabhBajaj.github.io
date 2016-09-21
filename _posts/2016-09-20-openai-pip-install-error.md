---
layout: post
title:  "OpenAI Gym: pip install error with pachi-py: command '/usr/bin/clang++' failed with exit status 1"
date:   2016-09-20 16:00:00 -0700
categories: openai gym ai
permalink: /openai-install-error
---



I started playing around with [OpenAI Gym](https://gym.openai.com/) recently which provides environements for training and testing reinforcement learning algorithms.

It provides a python package called gym, which contains environments for training the models. It requires a large number of python packages to be installed as dependencies. I ran into this error while running

```
    pip install gym[all]
```

from my environment

The error contains a large amount of C++ output with error messages like this:

```
    Build error by running "pip install -e .[all]" on OSX 10.11.5
    Building wheels for collected packages: pachi-py
    Running setup.py bdist_wheel for pachi-py ... error
    ld: warning: object file (pachi_py/build/lib/libpachi.a(network.c.o)) was built for newer OSX version (10.11) than being linked (10.6)
    ld: warning: object file (pachi_py/build/lib/libpachi.a(merge.c.o)) was built for newer OSX version (10.11) than being linked (10.6)
    ld: targeted OS version does not support use of thread local variables in _fast_srandom for architecture x86_64
    clang: error: linker command failed with exit code 1 (use -v to see invocation)
    error: command '/usr/bin/clang++' failed with exit status 1  
```

The package seems to be building binaries for OSX 10.5, which is incompatible with some recent libraries.

Here is the fix - export the MACOSX_DEPLOYMENT_TARGET env variable to 10.11 and run pip install again. This should fix the issue.

If you are using requirements.txt file:

```
    MACOSX_DEPLOYMENT_TARGET=10.11 && pip install -r requirements.txt
```

If you are downloading the [OpenAI Gym from Github](https://github.com/openai/gym) then run:

```
    MACOSX_DEPLOYMENT_TARGET=10.11 && python setup.py install
```

Let me know if that solution works for you.



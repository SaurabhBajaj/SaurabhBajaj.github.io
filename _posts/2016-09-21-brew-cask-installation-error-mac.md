---
layout: post
title:  "Mac OSX 10.11 Brew Cask installation Error: Unknown command: cask"
date:   2016-09-21 08:00:00 -0700
categories: setup brew
permalink: /brew-cask-error
---

Recently, in order to install CUDA for Tensorflow on my mac, I ran into an issue with setting up homebrew Cask.
[Brew Cask](https://github.com/caskroom/homebrew-cask) is a Homebrew extension for simplifying installation of GUI based mac applications.
While installing cask, I got this error:

{% highlight bash %}

mbp112:~ saurabhbajaj$ brew tap caskroom/cask
==> Tapping caskroom/cask
Cloning into '/usr/local/Library/Taps/caskroom/homebrew-cask'...
remote: Counting objects: 3427, done.
remote: Compressing objects: 100% (3408/3408), done.
remote: Total 3427 (delta 37), reused 451 (delta 13), pack-reused 0
Receiving objects: 100% (3427/3427), 1.15 MiB | 0 bytes/s, done.
Resolving deltas: 100% (37/37), done.
Checking connectivity... done.
Tapped 0 formulae (3433 files, 15M)


Sbajaj-mbp112:~ saurabhbajaj$ brew cask install cuda
Error: Unknown command: cask

{% endhighlight %}

I found the solution in one of the github issues on the [cask project](https://github.com/caskroom/homebrew-cask/issues/4667)

{% highlight bash %}

brew update
brew cleanup && brew cask cleanup
brew install caskroom/cask/brew-cask
brew untap caskroom/cask
brew tap caskroom/cask
brew install brew-cask

# Check that it works now
brew cask install anki

{% endhighlight %}
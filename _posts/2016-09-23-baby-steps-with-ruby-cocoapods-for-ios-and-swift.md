---
layout: post
title:  Baby steps with Ruby/CocoaPods for iOS and Swift
date:   2016-09-23 04:00:29
categories: iOS
---

Ruby was such a sweat to install. In the end I opted to use rvm to install and manage Ruby.

In terminal, type the following to download and install rvm (Ruby Version Manager)
```
  $ curl -sSL https://get.rvm.io | bash -s stable --ruby
```

check installed with `rvm list`

```
  $ rvm list
  rvm rubies
  =* ruby-2.3.0 [ x86_64 ]
  # => - current
  # =* - current && default
  #  * - default
```

if you switch versions, use the following to set a default
```
  $ rvm use 2.3.0 --default
```

In your terminal session you should then be able to check your rby and gem versions. For CocoaPods, you'll need at least version 2.2 of Ruby.
```
  $ ruby -v
  ruby 2.3.0p0 (2015-12-25 revision
  $ gem -v
  2.5.1
```

Install CocoaPods with
```
  $ sudo gem install cocoapods
```

cd into your project directly and ensure there is a PodFile. You can now run the pod install (or pod update) command
```
  $ pod install
```

Open up your project in xcode and you should be good to go!!

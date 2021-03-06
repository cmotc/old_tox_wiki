TOX FAQ
=======
 
Contents
========
 
1. What is Tox?
2. Where can I get Tox?
3. Tox
   1. Which encryption algorithms does Tox employ?
   2. Does Tox have plugin support?
   3. I want to contribute to the Tox project.
      1. I want to be a developer.
      2. I want to contribute in UI design/sound.
      3. Are there any other ways I can contribute?
   4. Can I use Tox over Tor?
4. Source
   1. Where do I get the Tox source code?
   2. How do I compile Tox?
5. Community
   1. Where can I find the latest Tox thread?
   2. Are there any other Tox threads/forums?

***

Answers
=======
 
1. What is Tox?
-----------------
Tox is a free (as in freedom) peer to peer messaging application that aims to 
replace skype.

2. Where can I get Tox?
-------------------------
It's not done yet.

3. Tox
--------

### 3.1. Which encryption algorithms does Tox employ?
Tox uses the encryption algorithms present in the NaCl crypto library.

### 3.2. Does Tox have plugin support?
Maybe.

### 3.3. I want to contribute to the Tox project

#### 3.3.1. I want to be a developer.
Join the IRC.

#### 3.3.2. I want to contribute in UI design/sound.
Join the IRC.

#### 3.3.3. Are there any other ways I can contribute?
Testing the application, reporting bugs and requesting features. Don't be 
scared to criticize something if you think it is done wrong.

### 3.4 Can I use Tox over Tor?
No, Tox uses UDP and the Tor network is TCP only.

4. Source
-----------

### 4.1. Where do I get the Tox source code?
The core library: https://github.com/irungentoo/ProjectTox-Core
Some front ends:
(None are in a usable state yet.)

### 4.2. How do I compile Tox?
Updated instructions found in [INSTALL.md](/irungentoo/ProjectTox-Core/blob/master/INSTALL.md) in the root of this repository.

You need to build and install `libsodium`.

On Mac OS X use [brew](http://brew.sh/) to install the dependencies

    brew update
    brew install libconfig libsodium

Then just cd in the repo and run

    mkdir build
    cd build
    cmake ..
    make

5. Community
--------------

## 5.1. Where can I find the latest Tox threads?


## 5.2. Are there any other Tox threads/forums?
# Notes on compiling "modern" software on Zaurus 

## Introduction

Given: Zaurus C3000 with installed [pdaXii3](http://www.users.on.net/~hluc/myZaurus/pdaxii13.html)

## Software

### Git

Using git 2.11.0

1. Need to patch Makefile. Find the line 

  ```
  BASIC_CFLAGS += -DHAVE_CLOCK_MONOTONIC
  ```
  
  and comment it out.

2. Compiling with the following command line

  ```
  NO_NSEC=1 NO_CURL=1 NO_PERL_MAKEMAKER=1 NO_PERL=1 NO_TCLTK=1 NO_GETTEXT=1 make
  ```

3. Install using 

  ```
  NO_NSEC=1 NO_CURL=1 NO_PERL_MAKEMAKER=1 NO_PERL=1 NO_TCLTK=1 NO_GETTEXT=1 install
  ```

---
title: "Thesis Code Installation"
date: 2021-10-09T14:57:16-05:00
draft: false
---

## Requirements

* Python 3
* Numpy
* [bitarray](http://pypi.python.org/pypi/bitarray/)
* [Weka](http://www.cs.waikato.ac.nz/ml/weka/) (3.6.8 and higher)
    * Java runtimes

## Installation

1. Install relevant Ubuntu packages:
    * python3
    * python3-numpy
    * python3-dev - required to compile and install bitarray
    * python3-setuptools - contains `easy_install`
1. Install pip and bitarray
    1. `easy_install3 pip`
    1. `pip install bitarray`
1.  Unzip Weka to some location.  I prefer a user's home directory.
1.  Update globalConfig.cfg with the full path of the Weka jar file and the Java runtime.
1.  Unzip the source code
1. Make the C++ library.  Run 'make' in the main source code directory.


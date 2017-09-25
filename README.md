# XL C/C++ for Linux, Community Edition

## Overview

This image contains the XL C/C++ for Linux, Community Edition compiler, it's installation dependencies, and some common development tools (vim-tiny, make, autoconf, automake).

For the details how this [ibmcom/xlc-ce](https://hub.docker.com/r/ibmcom/xlc-ce) image was built, see [xlc-community-edition-dockerfile](https://github.com/IBM/xlc-community-edition-dockerfile).

*Note:* Before installing additional packages using apt-get, update the package lists using 'apt-get update'. This image is currently based on [ppc64le/ubuntu](https://hub.docker.com/r/ppc64le/ubuntu/):
  
	FROM ppc64le/ubuntu:16.04


## Usage

To run this image interactively:  

	docker run -ti ibmcom/xlc-ce

The entrypoint is a script that will interactively display the license text and then configure the compiler.  
To automatically accept the license, add "-e LICENSE=accept" to your run command:  

	docker run -ti -e LICENSE=accept ibmcom/xlc-ce

You may also want to mount a directory that contains your source using -v. If your source is in /home/user/source, run:  

	docker run -ti -e LICENSE=accept -v /home/user/source:/source ibmcom/xlc-ce

Here is an example of using make to build an application at optimization level 3 \(-O3\), and then running it, in non-interactive mode:  

	docker run -e LICENSE=accept -v /home/user/source:/source --workdir /source ibmcom/xlc-ce bash -cx 'make CC=xlc CFLAGS=-O3 CXX=xlC CXXFLAGS=-O3 hello && ./hello'  
	+ make CC=xlc CFLAGS=-O3 CXX=xlC CXXFLAGS=-O3 hello  
	xlC -O3    hello.cpp   -o hello  
	+ ./hello  
	Hello C++ World!  


## Documentation

Documentation can be found in the [XL C/C++ for Linux documentation library](http://www.ibm.com/support/docview.wss?uid=swg27036675), product information can be found at [XL C/C++ for Linux](http://www.ibm.com/software/products/en/xlcpp-linux/).


## Providing feedback

XL C/C++ Community Edition is a no-charge product (unlimited license for production use) and does not include official IBM support.  
You can provide feedback at the [XL on POWER C/C++ Community Edition forum](http://ibm.biz/xlcpp-linux-ce).


## Contributions

All contributions to [xlc-community-edition-dockerfile](https://github.com/IBM/xlc-community-edition-dockerfile) require a comment in the pull request to indicate you accept the terms in [DCO.1.1.txt](https://github.com/IBM/xlc-community-edition-dockerfile/blob/master/DCO.1.1.txt) (example below):  
DCO 1.1 Signed-off-by: Ray Kivisto rkivisto at ca.ibm.com


## License

The Dockerfile is licensed under [Apache License, Version 2.0](https://github.com/IBM/xlc-community-edition-dockerfile/blob/master/LICENSE).  
XL C/C++ Community Edition is licensed under the [International License Agreement for Non-Warranted Programs](http://www14.software.ibm.com/cgi-bin/weblap/lap.pl?li_formnum=L-JYIP-AEMRYC).

DISCONTINUATION OF PROJECT.

This project will no longer be maintained by Intel.

Intel has ceased development and contributions including, but not limited to, maintenance, bug fixes, new releases, or updates, to this project. 

Intel no longer accepts patches to this project.

If you have an ongoing need to use this project, are interested in independently developing it, or would like to maintain patches for the open source software community, please create your own fork of this project. 
========================================================================
README for Intel(R)  ap patches Package

February 2018
========================================================================


Contents
========

Contains patches for kernel 4.9 and iproute2 v4.15.0

Overview
========

This kernel patch provide new tunnel type - L2 encapsulated to UDP
Patch for iproute2 provide functionality to create l2fou tunnel.

Configuration
=============

patch -p1 < PATCH_NAME

How to run
==========

On first machine
================

ip addr add 192.15.0.1/24 dev enp0s3
ip link set enp0s3 up
ip fou add port 5553 ipproto 143
ip link add l2fou_tun type l2fou remote 192.15.0.2 local 192.15.0.1 ttl 225 encap-sport 5553 encap-dport 5555
brctl addbr br0
brctl addif br0 l2fou_tun
ip link set dev l2fou_tun up
ip addr add 192.13.10.1/24 dev br0
ip link set dev br0 up

On second machine
=================

ip addr add 192.15.0.2/24 dev enp0s3
ip link set enp0s3 up
ip fou add port 5555 ipproto 143
ip link add l2fou_tun type l2fou remote 192.15.0.1 local 192.15.0.2 ttl 225 encap-sport 5555 encap-dport 5553
brctl addbr br0
brctl addif br0 l2fou_tun
ip link set dev l2fou_tun up
ip addr add 192.13.10.2/24 dev br0
ip link set dev br0 up


Encapsulated ICMP package. Example
==================================

Request: ports 5553 -> 5555

0000   52 54 00 48 d2 79 52 54 00 18 fe d0 08 00 45 00
0010   00 7e 11 b4 40 00 e1 11 07 99 c0 0f 00 01 c0 0f
0020   00 02 15 b1 15 b3 00 6a 00 00 1a c8 40 d6 c4 27
0030   9a 8a de 13 00 ef 08 00 45 00 00 54 69 aa 40 00
0040   40 01 3c e1 c0 0d 0a 01 c0 0d 0a 02 08 00 c8 55
0050   d2 06 00 0f d5 ba 87 d9 00 00 00 00 00 00 00 00
0060   00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
0070   00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
0080   00 00 00 00 00 00 00 00 00 00 00 00

Response: ports 5555 -> 5553

0000   52 54 00 18 fe d0 52 54 00 48 d2 79 08 00 45 00
0010   00 7e 2d 54 40 00 e1 11 eb f8 c0 0f 00 02 c0 0f
0020   00 01 15 b3 15 b1 00 6a 00 00 9a 8a de 13 00 ef
0030   1a c8 40 d6 c4 27 08 00 45 00 00 54 a4 33 00 00
0040   40 01 42 58 c0 0d 0a 02 c0 0d 0a 01 00 00 d0 55
0050   d2 06 00 0f d5 ba 87 d9 00 00 00 00 00 00 00 00
0060   00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
0070   00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
0080   00 00 00 00 00 00 00 00 00 00 00 00

Legal Disclaimer
================

THIS SOFTWARE IS PROVIDED BY INTEL"AS IS". NO LICENSE, EXPRESS OR
IMPLIED, BY ESTOPPEL OR OTHERWISE, TO ANY INTELLECTUAL PROPERTY RIGHTS
ARE GRANTED THROUGH USE. EXCEPT AS PROVIDED IN INTEL'S TERMS AND
CONDITIONS OF SALE, INTEL ASSUMES NO LIABILITY WHATSOEVER AND INTEL
DISCLAIMS ANY EXPRESS OR IMPLIED WARRANTY, RELATING TO SALE AND/OR
USE OF INTEL PRODUCTS INCLUDING LIABILITY OR WARRANTIES RELATING TO
FITNESS FOR A PARTICULAR PURPOSE, MERCHANTABILITY, OR INFRINGEMENT
OF ANY PATENT, COPYRIGHT OR OTHER INTELLECTUAL PROPERTY RIGHT.

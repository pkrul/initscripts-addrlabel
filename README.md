# initscripts-addrlabel

Add support to initscripts for IPv6 protocol address label management to
allow source address selection.


KERNEL ADDRESS LABELS
=====================

IPv6 address labels are used for source address selection;

By default, IPv6 addresses have 'lifetime' counters, which are valid_lft
and preferred_lft, normally set to 'forever'.

Because of this, the IPv6 interface address that was added most recently to
the kernel will be used as the source address for outbound connections.

To modify this behaviour, you have the option to use kernel address labels,
for controlling how networks (prefixes) are found from what interface
address.


RECOMMENDATIONS BEFORE USING ADDRESS LABELS
===========================================

Make sure the hosts you enter here are resolvable via DNS or /etc/hosts
You can also check name resolution with the `ip -6 -r addrlabel` command.
 
Set ''''multi on' in /etc/host.conf (should be default)


MORE INFORMATION
================

The '''ip-addrlabel(1), '''gai.conf(5), and '''getaddrinfo(3) manpages.
RFC 3484 and 6724 at http://ietf.org


INSTALLATION
============

From these sources, copy '''sysconfig/network-scripts/ifup-routes to
'''/etc/sysconfig/network-scripts/ overwriting the existing script.

Create a file '''/etc/sysconfig/network-scripts/addrlabel6-<dev>

Add prefixes to this file conform ip-addrlabel, without the 'ip' command 
itself, using a syntax similar to 'route-<dev>' etc. configuration files.


DEFAULT KERNEL POLICY TABLE
===========================

Below the policy table as described in RFC 6724, present in the kernel.
You can examine the present table with 'ip addrlabel' or 'ip addrlabel show'.

'''addrlabel add prefix ::1/128       label  0
'''addrlabel add prefix ::/0          label  1
'''addrlabel add prefix 2002::/16     label  2
'''addrlabel add prefix ::/96         label  3
'''addrlabel add prefix ::ffff:0:0/96 label  4
'''addrlabel add prefix 2001::/32     label  5
'''addrlabel add prefix fec0::/10     label 11
'''addrlabel add prefix 3ffe::/16     label 12
'''addrlabel add prefix fc00::/7      label 13


EXAMPLE 1
=========
You can add a label for your own IPv6 networks at, say label 20, and then
a label your IPv6 interface address at label 21, to ensure that this 
address is selected as source for outbound connections to this network.

'''addrlabel add prefix 2a01:ab12:cd34::/48       label 20
'''addrlabel add prefix 2a01:ab12:cd34:e56::1/128 label 21


EXAMPLE 2
=========
To connect from 2a01:: to 2001:: with a preferred source address, the label
of your 2a01-source needs a lower value than the label of the 2001-prefix..

'''addrlabel add prefix 2a01:ab12:cd34:e56::1/128 label 1


LIMITATIONS
===========

Yes


AUTHOR
======
Pieter Krul


THANK YOU
=========
Satoshi Shida San (http://sshida.com)


SEE ALSO
========
https://bugzilla.redhat.com/show_bug.cgi?id=583441



etcd
====

A highly available configuration store in the spirit of zookeeper.

Etcd allows storing data in a distributed hierarchical database with observation.

Usage
-----

We can deploy a single node easily with

 $ juju deploy cs:~hazmat/trusty/etcd

Add and capacity with:

 $ juju add-unit -n 2 etcd

Its recommended to run an odd number of machines as it has greater redundancy than
even number (ie. 4, you can lose 1 before quorum is lost, where as 5, you can 2).



Health
------

Health of the cluster can be checked by verified via juju run

 $ juju run --service=etcd ./health


Charm Notes
-----------

Wrt to cluster management, We can't do the natural bit in juju which
is to update a node with its set of peers.

Once a node has joined the cluster the state of the system is kept
entirely within the raft log.

Credits
-------

Original charm by @kapilt

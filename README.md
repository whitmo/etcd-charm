# Etcd

Etcd is a highly available distributed key value store that provides a reliable
way to store data across a cluster of machines. Etcd gracefully handles master
elections during network partitions and will tolerate machine failure, including
the master.

Your applications can read and write data into etcd. A simple use-case is to
store database connection details or feature flags in etcd as key value pairs.
These values can be watched, allowing your app to reconfigure itself when they
change.

Advanced uses take advantage of the consistency guarantees to implement
database master elections or do distributed locking across a cluster of workers.

Etcd allows storing data in a distributed hierarchical database with observation.

## Usage

We can deploy a single node easily with

    juju deploy cs:~kubernetes/trusty/etcd

Add and capacity with:

    juju add-unit -n 2 etcd

Its recommended to run an odd number of machines as it has greater redundancy than
even number (ie. 4, you can lose 1 before quorum is lost, where as 5, you can 2).


### Advanced Usage

This charm also supports a `proxy` relation, when using the ETCD cluster as a
discovery service for your own applications. Reference the
[upstream documentation](https://github.com/coreos/etcd/blob/master/Documentation/proxy.md)
to learn more about this feature of ETCD

    juju add-relation <service>:<relation> etcd:proxy


For each node in the cluster, you will receive a cluster-string that you can
use to point your application into the cluster and join the gossip.

## Health

Health of the cluster can be checked by verified via juju actions

    juju action do etcd/0 health
    <return response uuid>
    juju action fetch <uuid>


## Usage Caveats

This charm requires Leader Election, which is a feature of Juju >= 1.23.2. The
charm will panic and refuse to continue if the leader_election binary is not
found. Please take care when deploying this charm on older versions of Juju.

## Credits

The etcd charm was originally written by Kapil Thangavelu ([@kapilt](https://github.com/kapilt)).


#### Mantainers: 

The kubernetes team maintains this charm:
  - Whit Morriss &lt;whit.morriss@canonical.com&gt;
  - Charles Butler &lt;charles.butler@canonical.com&gt;
  - Matt Bruzek &lt;matthew.bruzek@canonical.com&gt;


## Upstream Project Information

- [Using ETCD](https://coreos.com/using-coreos/etcd/)
- [ETCD Getting Started Guide](https://coreos.com/docs/distributed-configuration/getting-started-with-etcd/)
- [ETCD Issue Tracker](https://github.com/coreos/etcd)

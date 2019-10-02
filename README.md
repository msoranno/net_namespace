
# Using network namespaces and a virtual switch to isolate servers

Using network namespaces and a virtual switch to isolate servers

## Build

- two network namespace (in this case, we can think of each network namespace as a different computer)
- two veth pairs (we can think of each pair as two ethernet cards with a cable between them)
- a bridge device that provides the routing to these two namespaces (we can think of this device as a switch)

### create namespace
```
ip netns add namespace1

ip netns add namespace2

sudo ip netns list

# This 2 files were created

[sp81891@oc2157818656 ~]$ ls -alrt /var/run/netns/
total 0
drwxr-xr-x. 44 root root 1400 Oct  2 15:51 ..
-r--r--r--.  1 root root    0 Oct  2 15:51 namespace1
drwxr-xr-x.  2 root root   80 Oct  2 15:51 .
-r--r--r--.  1 root root    0 Oct  2 15:51 namespace2

# Now we can take advantage of the isolated namespace

[sp81891@oc2157818656 ~]$ sudo ip netns exec namespace1 ip add
1: lo: <LOOPBACK> mtu 65536 qdisc noop state DOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    
    [sp81891@oc2157818656 ~]$ sudo ip netns exec namespace1 ip link
[sudo] password for sp81891: 
1: lo: <LOOPBACK> mtu 65536 qdisc noop state DOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00

```

### Create two interfaces type veth (virtualk internet interface)

- Create the two pairs.
ip link add veth1 type veth peer name br-veth1
ip link add veth2 type veth peer name br-veth2

- Associate the non `br-` side with the corresponding namespace
ip link set veth1 netns namespace1
ip link set veth2 netns namespace2


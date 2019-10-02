
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

[sp81891@oc2157818656 ~]$ ls -alrt /var/run/netns/
total 0
drwxr-xr-x. 44 root root 1400 Oct  2 15:51 ..
-r--r--r--.  1 root root    0 Oct  2 15:51 namespace1
drwxr-xr-x.  2 root root   80 Oct  2 15:51 .
-r--r--r--.  1 root root    0 Oct  2 15:51 namespace2
```

###

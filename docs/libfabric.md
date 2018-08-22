# Derecho with Libfabric
[OFI Libfabric](https://ofiwg.github.io/libfabric/) is a **remote direct memory access (RDMA)** communication library. It unifies the distinct programming APIs of different RDMA hardware with a thin layer. We are working on Derecho porting to libfabric so that it can support applications in more environments. Specifically, libfabric can work on TCP/IP network without special RDMA hardware, making Derecho available to almost all cloud application.

## Throughput
We tested the derecho atomic broadcast throughput with various hardware. (Explanation to be added later.)
<table>
 <tr>
  <td>
   <img src="https://github.com/Derecho-Project/blog/blob/master/figs/libfabric-throughput/ib100g.pdf.png" width="480"/> 
  </td>
  <td>
   <img src="https://github.com/Derecho-Project/blog/blob/master/figs/libfabric-throughput/eth100g.pdf.png" width="480"/>
  </td>
 </tr>
 <tr>
  <th>(a) RDMA w/ 100Gb Infiniband</th>
  <th>(b) TCP w/ 100Gb Ethernet</th>
 </tr>
 <tr>
  <td>
   <img src="https://github.com/Derecho-Project/blog/blob/master/figs/libfabric-throughput/ipoib.pdf.png" width="480"/> 
  </td>
  <td>
   <img src="https://github.com/Derecho-Project/blog/blob/master/figs/libfabric-throughput/eth10g.pdf.png" width="480"/>
  </td>
 </tr>
 <tr>
  <th>(c)TCP w/ 100Gb IPoIB</th>
  <th>(d)TCP w/ 10Gb Ethernet</th>
 </tr>
 <tr>
  <td colspan="2" align="center">Figure 1. Derecho Atomic Broadcast Throughput</td>
 </tr>
</table>

## Latency
We tested the derecho atomic broadcast latency with various hardware. (Explanation to be added later.)
<table>
 <tr>
  <td>
   <img src="https://github.com/Derecho-Project/blog/blob/master/figs/libfabric-latency/lat-1k-lf-vs-verbs.pdf.png" width="480"/> 
  </td>
  <td>
   <img src="https://github.com/Derecho-Project/blog/blob/master/figs/libfabric-latency/lat-1m-lf-vs-verbs.pdf.png" width="480"/>
  </td>
 </tr>
 <tr>
  <th>(a) 1KB message latency: libfabric vs ibverbs</th>
  <th>(b) 1MB message latency: libfabric vs ibverbs </th>
 </tr>
 <tr>
  <td colspan="2" align="center">Figure 2. Derecho Atomic Broadcast Latency</td>
 </tr>
</table>

## Comparison with Existing Systems

<table>
 <tr>
  <td>
   <img src="https://github.com/Derecho-Project/blog/blob/master/figs/libfabric-comparison/apus-derecho-ib100g-ops.png" width="480"/>
  </td> 
  <td>
   <img src="https://github.com/Derecho-Project/blog/blob/master/figs/libfabric-comparison/apus-derecho-ib100g-thp.png" width="480"/>
  </td>
 </tr>
 <tr>
  <th>(a) Atomic Broadcasts: Derecho vs APUS </th>
  <th>(b) Throughput: Derecho vs APUS </th>
 </tr>
 <tr>
  <td colspan="2" align="center">Figure 3. Derecho vs APUS with Three Nodes over 100G InfiniBand</td>
 </tr>
 <tr>
  <td>
   <img src="https://github.com/Derecho-Project/blog/blob/master/figs/libfabric-comparison/zk-paxos-derecho-eth100g-ops.png" width="480"/>
  </td> 
  <td>
   <img src="https://github.com/Derecho-Project/blog/blob/master/figs/libfabric-comparison/zk-paxos-derecho-eth100g-thp.png" width="480"/>
  </td>
 </tr>
 <tr>
  <th>(c) Atomic Broadcasts: Derecho(TCP) vs libpaxos and zookeeper </th>
  <th>(d) Throughput: Derecho(TCP) vs libpaxos and zookeeper </th>
 </tr>
 <tr>
  <td colspan="2" align="center">Figure 4. Derecho vs LibPaxos and ZooKeeper with Three Nodes over 100G Ethernet</td>
 </tr>
</table>

<P>Using the same cluster on which we evaluated Derecho, we conducted a series of experiments
using competiting systems: APUS, LibPaxos, ZooKeeper. All three systems were configured to run
in their atomic multicast (in-memory) modes. APUS runs on RDMA, hence we configured Derecho
to use RDMA for that experiment. LibPaxos and ZooKeeper run purely on TCP/IP, so for those
runs, Derecho was configured to map to TCP.
<P>
The comparison with APUS can be seen in Figure 3.  We focused on a 3-member group, but
experimented at other sizes as well; within the range considered (3, 5, 7) APUS performance was
constant. APUS does not support significantly larger configurations. As seen in these figures,
Derecho is faster than APUS across the full range of cases considered. APUS apparently is based
on RAFT, which employs the pattern of two-phase commit discussed earlier, and we believe this
explains the performance difference.
<P>
Comparisons of Derecho durable Paxos over TCP with Zookeeper and LibPaxos are seen in
Figure 4 (Zookeeper does not support 100MB writes, hence that data point is omitted). Again,
we use a 3-member group, but saw similar results at other group sizes. LibPaxos employs the
Ring Paxos protocol, which is similar to Derecho’s protocol but leader-based. Here the underlying
knowledge exchange is equivalent to a two-phase commit, but the actual pattern of message passing
involves sequential token passing on a ring.
<P>
The comparison with ZooKeeper is of interest, because here there is one case (10KB writes)
where ZooKeeper and Derecho have very similar performance. Derecho dominates for larger writes,
and of course is substantially faster over RDMA. Here, the issue is that
the LibFabrics mapping to TCP itself is somewhat slow for small writes, hence what we are seeing
is not so much that ZooKeeper is exceptionally fast, but rather that the underlying communications
technology is not performing terribly well, and both systems are bottlenecked. More broadly,
Derecho’s “sweet spot,” for which we see its very highest performance, involves large objects, large
replication factors, and RDMA hardware. The existing systems, including APUS, simply do not run
in comparable configurations and with similar object sizes.
<P>
we should note that were we to compare Derecho’s peak RDMA rates for large
objects in large groups with the best options for implementing such patterns in the prior systems
(for example, by breaking a large object into smaller chunks so that ZooKeeper could handle them),
Derecho would be faster by factors of as much as 500x. We omit such cases because they raise
apples-to-oranges concerns. Nonetheless, modern data center replication scenarios actually do
involve replication of large objects in large groups. Thus the inability of existing systems to support
such update patterns and to leverage the hardware is highly relevant.
</P>

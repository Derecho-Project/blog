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
 

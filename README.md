# NEWS
## 13-Nov-2018: Many improvements
commit: ba8e0e11528812ee97737c4d3f433ed4a11122a1
- Derecho now supports running multiple instances in a same OS
- Improved recovery code
- Added object store

## 21-Aug-2018: Derecho Libfabric is Merging to master.
We are working on the merge of libfabric and master branch. An initial merge is available in master. Please do not use that version until merge is finalized. We also updated the [document](https://github.com/Derecho-Project/blog/blob/master/docs/libfabric.md) to show the performance comparisons against existing systems.

## 28-Jun-2018: Geo-Distributed Derecho Roadmap.
In the next couple of months, we plan to try Derecho in geographically distributed environment.
1) Testing Derecho + libfabric/TCP in wide area network.
2) A storage prototype with Derecho replicating data geographically.

## 28-JUN-2018: A Glance of Derecho with Libfabric.
Derecho worked with ibverbs API. We are working on a new version using Libfabric, which adapts Derecho to [more environments](https://github.com/ofiwg/libfabric): TCP/IP(no RDMA hardware required), Intel Omni-Path, Cray GNI, etc... We currently tested it with TCP/IP, InfiniBand, and RoCE. More details are [here](https://github.com/Derecho-Project/blog/blob/master/docs/libfabric.md).

## 28-JUN-2018: Hello, Derecho Blog!
This blog hosts Derecho project news. 

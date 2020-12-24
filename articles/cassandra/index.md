# Introduction to Apache Cassandra

> Apache Cassandra is an open-source, distributed, decentralised, elastically scalable NoSQL database. 

Cassandra is a distributed database, it can run on multiple machines while appearing to the user as a single node. Cassandra really shines when it is used on multiple-machines, because it is optimized for high performance across
multiple data-center racks and even multiple data-centers across the globe. In other databases as you scale, some nodes need to be setup as primary nodes and some as secondary replicas, Cassandra is decentralized so there is no concept of primary and secondary, every node is identical. Decentralization is pretty useful as it is easy to use than primary-secondary and there is no single point of failure. Scalability is a feature of a system where it can serve more requests without a degradation in performance. Cassandra offers elastic scalability where we can scale up or scale down seamlessly without restarting the processes or much diturbance.    

Line about nosql

## Installing Cassandra

Follow the guide [here](https://cassandra.apache.org/doc/latest/getting_started/installing.html)


## When to use cassandra?

Cassandra is suitable for your project is your project has 

- Large Deployment

If you are deploying on a single node, then Cassandra's full potential may not be realised. Familiar DBMS software can be used for a single node deployment as they are easier to run on a single machine. But for projects where you have several nodes Cassandra might be very good.

- Lots of writes

Cassandra is optimized for increasing the thoroughput on writes. Applications which have huge write volumes with many concurrent threads can benfeit from this, applications which deal with stock market data is an example.

- Multiple Data Centers

Cassandra has out-of-the box support for geographical distribution of data. Cassandra can be easily configured for multiple data centers.


## Architecture

## CQL

## Application development with Cassandra

## Conclusion

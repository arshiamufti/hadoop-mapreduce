
## MapReduce

Say we had a massive file. Processing it serially (beginning to end) could take
a long time. What if we broke it down into chunks and then processed each chunk
in parallel? In other words, we can break a file into chunks and define
**mappers** that process the data in parallel. The output of the mappers is called
"the intermediate output" where each record in the output is in the form of a key,
value pairs. When they're done, we can define
**reducers** that take the data created by the mappers and aggregate it. The
movement of the data created by the mappers to the reducers is the "shuffle"
phase. The "sort" phase is when the reducers organize the records before
aggregating the results.

**IRL example**: Say we want to count the # of clicks per product on a website
over the past 10 years. The data we have is in the form of (product name, time
stamp) in a big big big file. The **mappers** each work on smaller chunks of
this file by processing each chunk serially and creating buckets of products.
The output of each mapper would be list of keys (product names) and values
(number of clicks for each product). This data is then given to the **reducers**.
Each reducer is responsible for counting the # of clicks for one or more products.
For example, reducer #1 could aggregate the clicks for products #1 and #2, reducer
#3 could do the same for product #3 and so on). So reducer #1 would, from each mapper,
collect the data for the products (#1 and #2) that it was responsible for (**shuffle**).
The reducers then **sort** the records they have before aggregation. Then for each product,
they simply add up the number of clicks for all the products.

Note that there is no rule as to how reducers are assigned "stores" in this case.

### Segue: Partitioning data

Wisdom taken from [yahoo dot com](https://developer.yahoo.com/hadoop/tutorial/module5.html#partitioning).

- Partitioning is the process of determining which reducer instance will receive which intermediate keys and values.
- It is the **mapper**'s job to figure out for all its output (which is in the form of key-value pairs, which reducer will receive each record. For each key, regardless of which mapper generated it, the destination partition **must be the same**.
- For performance reasons, mappers should not have to talk to one another to determine which partition a key belongs to. In other words, mappers should be able to partition data *independently*.
- Hadoop uses an interface called the _Partitioner_ to determine whiich partition a record (key-value pair) will go into. A single partition refers to all the key-value pairs that will be sent to a single reduce task. Before the job starts, Hadoop MR determines how many partitions to divide the data into. If 20 reducers have to run, 20 partitions must be filled. *The Partitioner_ determines a method that has to be defined

```java
// input: a key, a value and the # of partitions to split the data across
// output: a number in the range [0, numPartitions] indicating which partition to
// send the key and the value to
// for keys k1 and k2, k1 == k2 => getPartition(k1, _, n) = getPartition(k2, _, n)
int getPartition(K key, V value, int numPartitions);
```

- There is a default Paritioner implementation, `HashPartitioner`. For randomly distributed data, it creates paritions of roughly equal size. If the data's skewed, it probably won't produce a good set of partitions. For example, if 80% of your eys are 0 and the rest are uniformly distributed among 1...9, and you have 2 reducers, you probably don't want to assign reducer #1 the keys 0...4 and reducer #2 the keys 5...9 since work is split unevenly among reducers. A better distribution would be assigning reducer #1 the key 0 and reducer #2 the rest of the keys.
- You can write your own *Partitioner* implementation, tailored to the data you are aggregating in your MapReduce or Spark job.

### Daemons

There are a set of daemons running on each machine in the cluster. When a MapReduce job is written, it's submitted to a **job tracker** which splits the work into mappers and reducers. These mappers and reducers then run on the cluster nodes. Running the actual map and reduce tasks is handled by the **task tracker** (another daemon). The task tracker runs on each data node. The Hadoop framework then makes the mappers work only on the data of their machine which will reduce network traffic since the mappers run independently of each other. In some cases, the task tracker on a machine might be too busy to process data, in which case, an idle machine processes it (so data would have to be streamed over a network to the idle machine)--this rarely happens.

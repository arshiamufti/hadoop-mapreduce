Files are stored in thw Hadoop Distributed File System (HDFS).

Say we have a 150 MB file, data.txt. The file is split into 64 MB file and given
a unique name. Here we have 2 64 MB blocks and one 22 MB file. As the file is
uploaded to HDFS, **each file is uploaded to a node in the cluster**. There is a
daemon running on each machine in the cluster, called the data node. There is a
separate machine (also running a daemon) called the name node that stores
**metadata**.

If we had only one data node, a disk failure on the name node would result in a
huge loss of data (we'd have all the blocks of the files, but we wouldn't know
which block belonged to which file). Similarly, a disk failure on a data node
would result in the loss of parts of files on the cluster. Which is bad.

To solve this problem, Hadoop replicates each block three times as its stored in
HDFS. So a given block may be replicated and stored on three random nodes. SO if
a node fails, it's okay because we have two other copies. The name node arranges
for new copies to fail. For a long time, there was only one name node (which was
a single, critical point of failure in the FS). So what people did was configure
the nn to store data not jsut on its own hard drive, but also somewhere on a
network file system. That way, a copy of the metadata exsits on the network.
These days, there's **two** name nodes. The active node works as before. The
standby takes over if the active one fails.

## Working with HDFS on the command line

Commands that interact with hdfs start with `hadoop fs`

- `hadoop fs -ls` - list of files in the current directory on the hadoop cluster
- `hadoop fs -put file.txt` - takes a local file and places it into hdfs and
  give it the same file name`
- `hadoop fs -tail foo.txt` - unix `tail` but for a file in the hadoop fs
- `hadoop fs -v foo.txt newfoo.txt`
- `hadoop fs -rm newfoo.txt`
- `hadoop fs -mkdir blah`

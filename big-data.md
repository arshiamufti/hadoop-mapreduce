## Big Data

### Definition

As newer technologies are created, data is created faster and faster. IBM
estimated that 90% of the world's data was created in the last two years alone.

**How do you define big data?** Most people consider a few TBs of data to be
"big data" but there are cases where Hadoop and MapReduce is used with a few GBs
of data with good results as well. A good definition is "data that is too big to
be process on a single machine" or "data that is too large and/or complex that
it's awkward to work with using standard statistical software".

### Aspects of big data

volume (refers to the size of the data being dealt with),
variety (data often comes in from different sources and in different formats),
velocity (the fact that data is generated quickly and needs to be processed
quickly as well).

**Volume**. The cost of data was super expensive a few decades ago. Has become
**much** cheaper now. As the volume of the data grows, we need a cost
efficient way to store it reliably as well as read from/write to it.
Performing a task efficiently involves tweaking a lot of variables, so the
more information we are able to store and process, the better decisions we'll
be able to make.

**Variety**. For a long time, people used databases such as SQL or data
warehouses from Oracle or IBM to store their data. The problem--to store data in
that way, the data needs to fit a pre-defined schema (tables). Data these days
is usually unstructured or semi-structured. **Unstructured**--data is in
**different formats**. Example, a transaction can be made through a credit
card, cheque etc, and we want to be able to store it in its original
format. We want to store the original format so we don't lost any
information. The data can then be converted to other formats from the
initial format. Hadoop is useful because it doesn't care what format the data
is in, so we can store it in its original form (unlike a traditional
database) and manipulate/reformat it later.

**Velocity**. We need to be able to accept, store the data as it arrives,
without dropping any.

### Core Hadoop

Consists of a way to store data (HDFS) and process it (MapReduce). Data is
split up and stored in a collection of machines (called a cluster). Then, when
we want to **process** the dtaa, we process it from the location it's
actually stored in (rather than retrive it from a central server). More
machines can be added to the cluster as the amount of data grows.

### Hadoop Ecosystem

Other software has grown around software since it was created, to make Hadoop
easier to use. Writing MapReduce code isn't super straightforward. You need to
know how to program in Java or some other language. It would be nice to be able
to access data using SQL queries. Other OS projects have been developed to make
it easier to acess data without writing mappers and reduces. Examples: Hive,
Pig.

- **Hive** allows you to write "select * from..."-esque data which is
  interpreted and then converted to MR code which then runs on the cluster.
- **Pig** allows you to write code to analyze data using a scripting language.
-**Impala** was developed as a way to access data using SQL by **directly**
accessing the data in HDFS (without an intermediate conversion to MR code
first). Impala is optimized for **low latency queries**. Hive is optimized for
running long batch processing jobs.
- **Sqoop** takes data from a relational database and puts it in HDFS so it can
  be processed along with other data on the cluster.
- **Flume** ingests data as its generated by external system and writes it to
  the cluster.
- **Hbase** is a real time database built on top of HDFS.
- **Hue** is a graphical frontend for the cluster. **Mahout** is an ML library.
  **Oozie** is a workflow management tool.

**CDH** takes all the key ecosystem projects and packages them together to
that installation is easy. Components are tested to make sure they're
compatible with each other. Developed by Cloudera, open source.

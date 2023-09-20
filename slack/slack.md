

### Random Sampling

Hi there, we do have a use case were we want to generate a sample from our data in influx. According to the documentation, there are currently selectors for min, max, first, and last. Are there any plans to add a random selector to it (or maybe this already exists but is not documented yet)?

Marco Neumann, 5 days ago

For SQL, you have two options depending on the type of sampling you wanna perform:

1) probabilistic sampling: SELECT ... FROM ... WHERE random() < p (p being your probability in [0, 1))

2) fixed-size sampling: For that we don't have a good story yet. You CAN use SELECT ... FROM ... ORDER BY random() LIMIT n (n being the maximum result size) but it will NOT perform very well. It basically reshuffles the entire data just to pick a few entries from the top. We know that this is something that people wanna do, so my guess is that eventually we will improve that (either by special optimizing that particular plan or by offering you a different tool / mechanism / query pattern to achieve that).

### 3.0 Data Model
by Rick Spencer

We started by rethinking the data model from the ground up. While we retain the notion of separating data into databases, rather than persisting time series, 3.0 persists data by table, where “table” is another name for “measurement” in InfluxDB 1.0 and 2.0 parlance.

Each table is sharded on disk by day and persisted in the Parquet file format. Therefore if you visualize the data on disk, it looks like a set of Parquet files, where each one represents one day of data for a single measurement. There is a caveat: each file is limited to 100 megabytes, so there may be multiple files per day for heavy users.

<graphic here? Show tables sharded by day>

InfluxDB 3.0 retains the notion of tags and fields, however, they play a different role in 3.0. Namely, a unique set of tag values and a time stamp identifies a row so that it can be updated by an UPSERT on write.

Understanding the new data model allows us to go on to discuss ingest efficiency and compression.

Alternate Partitioning Options

InfluxDB 3.0 is designed to perform analytical queries (i.e. queries that summarize across a large number of rows) and the default partitioning scheme is optimized for this. However, for some measurements, the user may wish to always query a subset of tag values. For example, their queries are always for a single customer id or sensor id. TSM is very well suited to such queries because that is how the data is indexed and persisted on disk, but not so with InfluxDB 3.0. As such, using the default partitioning scheme, users may experience a regression in performance for these specific queries.

The solution to this is “custom partitioning.” Custom partitioning allows the user to define a partitioning scheme based on tag keys and tag values, and also tweak the time range for each partition.

In this way, users can come close to achieving the same query performance for these specific query types in 3.0 while retaining the benefits of efficient ingest and compression.

### Any chance Influx 3.0 will support DML (UPDATE/DELETE) of data with SQL?

paul dix response:   
I don't think in the near or medium term. Can you say more about why you're asking about those features via SQL specifically?

Thanks
@pauldix
 - only exploratory at this stage, new to Influx, interested in 3.0/IOx and trying to understand how SQL fits in the picture. We have a requirement to periodically update our time series data so wondered if we can use standard SQL to do so

### When reading a parquet file produced by IOx, how can tags and fields columns be identified?

9/20/23

would you mind helping with this question. I haven't come across how we represent tag information within parquet. Is this specifically represented within parquet or within the metadata stored in postgres?

dom
  2 days ago
Hey - yeah it’s metadata that’s stored in the catalog, not the file itself

dom
  2 days ago
That metadata is queryable from the IOx routers if you’re running your own servers

Hans Matlof
  1 day ago
What I'm interested in is to determine if a column is a field or a tag when reading the parquet files in Spark using pyspark, so what you are telling me is that I should rewrite the loading of the Parquet files to interact with those IOx routers so they can return the info? When reading the parquet files with pyarrow.parquet there are some metadata which specify if a column is a field or a tag, but I assume this is part of the arrow schema, not directly a metadata of the parquet file. So maybe instead of interacting with the IOx router it could be possible to read the arrow schema from the parquet metadata and extract the info from there. Not straightforward but doable for who is willing to rewrite a loader in pyspark.

dom
  1 day ago
There is some metadata within the arrow schema in the file yeah - I don’t know if/how you’d access that from Spark though without writing some code as you suggest though.

Hans Matlof
  1 day ago
So what is the typical process you envision for being able to analyze the data from IOx parquet files using spark?

dom
  1 day ago
Spark doesn’t share the same concept of tags from Influx - typically the data would be treated as “just a table of data” and used as is - what’s your use case for needing to distinguish between Influx tag/fields in Spark?

Hans Matlof
  1 day ago
tags are metadata, so ideally if using something like flint (https://github.com/twosigma/flint) they should not be repeated when converting a series in a TimeSeriesDataFrame, so there is a need to differentiate tags and fields. Maybe that can be done manually on a case by case basis though, but having the possibility to automate the process seems better for making some spark jobs more generic.

Adam Thornton
  1 day ago
For whatever it's worth, Avro has the same model, in that it doesn't have a concept of tag versus field.
So in the plugins/parsers/avro directory of telegraf you can find how we (by which I mean Rubin Observatory--although this is upstreamed, I don't speak for Influxdata, just for that parser) do it, which is by defining a parser config that names the tags, and an optional one that names the fields (default: if it's present, and it's not a tag, it's a field).  I don't think there's an automated way to tell, but of course in your use case, if you have some conventions, you can automate production of the configuration files.  That's what we do--we're running under Kubernetes, and so we just produce our consumers and their configmaps by ranging over a value with Helm (which is a fancy way of saying, there's a for loop, and we produce a bunch of very similar parser configurations by using the index variable).

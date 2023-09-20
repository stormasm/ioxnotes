

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

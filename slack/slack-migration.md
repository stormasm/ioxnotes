

### Migration Issue: Early September 2023

Hi there, we are currently running into performance issues with our Influx. In our query we aggregate our data per month over the last year; we have a resolution of 10 minutes (= roughly 53k data points). Influx needs 20s for this query - on the old TSM engine this query needed just a few hundred milliseconds. 20s is not an acceptable runtime for our productive system and we need to find a way to fix this. Is Influx aware of these performance issues and working on it? Could we optimize our query to run faster? We need someone to help us here, please. :slightly_smiling_face:
16 replies


Jay Clifford
:kubo_multi:  26 days ago
Hi
@Cindy Perscheid
, could you provide your current query and we could take a look?


Jay Clifford
:kubo_multi:  26 days ago
Hi
@Cindy Perscheid
 just going to keep are DM chat going here since it's good visibility for our Dev team. So you said via DM you are using the  V2 query API and SQL queries. Are you injecting the SQL via flux in that case?


Cindy Perscheid
  26 days ago
we are using this iox.sql(bucket: "%s", query: query)  to generate the flux query (that was how it was recommended when we migrated a couple of weeks ago)


Jay Clifford
:kubo_multi:  26 days ago
Okay, so the hope is to eventually move across to V3 libaries when it's possible to do so. We unfortunately cannot account for the performance of the V2 API use InfluxDB 3.0. What language are you utilising within your application perhaps I could help with the migration there?
:face_with_rolling_eyes:
1



Cindy Perscheid
  26 days ago
We use Python and write most of the queries in SQL. We still have one large query in Flux where we are actually not sure if we can already construct an SQL equivalent (since there is still functionality missing)


Jay Clifford
:kubo_multi:  26 days ago
Would it not be possible to do the further processing once it reaches python? In most cases it would be pretty perfomant to run the analysis with the pyarrow library or pandas . We could also try influxql via Apache flight aswell to see if this also fills the void (edited)


Cindy Perscheid
  26 days ago
yes of course, but we can do all this, its just a) upsetting and b) way more work and blocks us. we are constantly dealing with firefighting Influx-related stuff because some behavior again just changed without communication.


Cindy Perscheid
  26 days ago
so just to clarify: the only option we have now is to move to the v3 client because Influx does not account for the performance of the v2 API any longer?


Jay Clifford
:kubo_multi:  26 days ago
Here is the current V3 client library: https://github.com/InfluxCommunity/influxdb3-python
I am currently one of the main contributors so can help out. Completely understand that V3 had been a heavy migration strain. It's been a huge shift in technology to accommodate for known issues in TSM.
Our recommendation based on the V3 documentation is to make use either the new Flight SQl query method or use the V1 API to run perfomant queries against 3.0. We have stated the the V2 query API still exists in serverless but many will see a drop in performance since we do not have a native implementation of flux in Apache Datafusion.
The V2 write API continues to V3 as normal so need for changes there
Happy to help with the migration process and any questions. My apologies again this has been your experience with the closure of your region. Thank you for continuing to persist. If you wanted to move back to 2.x region we could look into this for you. (edited)
InfluxCommunity/influxdb3-python
Python module that provides a simple and convenient way to interact with InfluxDB 3.0.
Website
https://www.influxdata.com
Stars
11
Added by GitHub


Cindy Perscheid
  26 days ago
 We have stated the the V2 query API still exists in serverless but many will see a drop in performance since we do not have a native implementation of flux in Apache Datafusion.
WHEN has this been communicated?!?


Jay Clifford
:kubo_multi:  26 days ago
We have many times within the forums over questions with regards to flux within 3.0. It is one of the main reasons we have not included Flux as primary query method within the new documention for serverless. (edited)


Jay Clifford
:kubo_multi:  26 days ago
I am extremely sorry this has not been officially stated outright via the documentation however. I will report this back to the team.
I am still more than happy to support you through the move to the new library if you need it. Just let me know.


Cindy Perscheid
  26 days ago
provide us with an async client :smile:


Jay Clifford
:kubo_multi:  26 days ago
I will add it as an issue and check what we can be done in the underlining pyarrow library.


Cindy Perscheid
  26 days ago
we actually tried out some queries with the new flightsql client (in non-async mode). the query is faster, but still way too slow. from our tests we could also observe large performance differences depending on for what time we request; e.g. if we request data from the current year that query is faster than the exact same query executed for data from the year before (12s vs. >40s runtime, respectively. again for 53k data points). We have no reliable benchmark on this though, it’s just what we could observe from trying out. Could the “age” of the data or data gaps have an impact on the runtime performance?


rickspencer3
  26 days ago
Hi Cindy. Can you please share your query here so I can investigate this more?

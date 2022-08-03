
So it breaks on the commit:

* [run many compact partitions in parallel](https://github.com/influxdata/influxdb_iox/pull/5230)

Now I can figure out what to do...



### Works here

* [July 12](https://github.com/influxdata/influxdb_iox/pull/5110) :
 [pr5110](https://github.com/influxdata/influxdb_iox/commit/ad4ea13e9d3c51cab1febc770c51d22a05855ac6)
 
 * [July 19](https://github.com/influxdata/influxdb_iox/pull/5134) :
 [pr5134](https://github.com/influxdata/influxdb_iox/commit/b8d9799a268634b4a8226554c8950a480b9760b0)
 
 * [July 25](https://github.com/influxdata/influxdb_iox/pull/5206) :
 [pr5206](https://github.com/influxdata/influxdb_iox/commit/d05f383a986414c6576a2abcb2a34fe5bb1a8b25)
 
 * [July 26](https://github.com/influxdata/influxdb_iox/pull/5210) :
 [pr5210](https://github.com/influxdata/influxdb_iox/commit/de22b6b080b66d89f0c44cccdf1b9dd7ed124b9f)
 
 * [July 27](https://github.com/influxdata/influxdb_iox/pull/5225) :
 [pr5225](https://github.com/influxdata/influxdb_iox/commit/7eebe061a69b20430d911383a199b381c4e4fbfa) **fix: reduce log verbosity for found compaction candidates message**
 
 ### This is where it breaks pr5230
 
 * [July 27](https://github.com/influxdata/influxdb_iox/pull/5230) :
 [pr5230](https://github.com/influxdata/influxdb_iox/commit/fcce00bf0921622eb1d1334865b2b8fc5fa6f7ed) **feat: run many compact partitions in parallel**
 
 * [July 28](https://github.com/influxdata/influxdb_iox/pull/5229) :
 [pr5229](https://github.com/influxdata/influxdb_iox/commit/9215a534d0fd241442c40db995884b6d899800eb)
 
 * [July 28](https://github.com/influxdata/influxdb_iox/pull/5235) :
 [pr5235](https://github.com/influxdata/influxdb_iox/commit/0e9695f20273818502e6c8b8f49c02f6449aca17)
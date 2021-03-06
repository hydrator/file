File Batch Sink
===============

Description
-----------
Batch sink that writes to Hadoop compatible File Systems such as HDFS, S3, GCS (Google Cloud Storage), etc.
Each record is written out as text by delimiting record fields with a configurable delimiter.
Non-string fields will be converted to strings using their ``toString()`` Java method, 
so fields should be limited to the string, long, int, double, float, and boolean types. 
Fields cannot have null values. 


Use Case
--------
This sink is used when you want to write text files to a Hadoop FileSystem.
For example, you may want to run a daily job that reads the contents of a database,
then dumps the contents onto a Hadoop FileSystem. 


Properties
----------
**referenceName:** This will be used to uniquely identify this sink for lineage, annotating metadata, etc.

**path:** The directory to write to. For example, ``hdfs://mycluster.net:8020/my/desired/location``.

**suffix:** Time suffix to append to the path for each run of the pipeline. For example,
``YYYY-MM-dd-HH-mm`` will take the start time of the pipeline run, convert it into
year-month-day-hour-minute format, and append that to the path to get the final output directory.
If not specified, no suffix is used.

**delimiter:** Delimiter used to concatenate record fields. Defaults to a comma ','. (Macro-enabled)

**jobProperties:** Advanced feature to specify any additional properties that should be used with the sink,
specified as a JSON object of string to string. These properties are set on the job at runtime. (Macro-enabled)

Example
-------
This example writes to the Hadoop FileSystem with its namenode running on ``mycluster.net``,
to the ``/etc/accesslogs/YYYY-MM-dd-HH-mm`` path. For example if the run was scheduled to
run midnight on new years day 2016, the pipeline would write to ``/etc/accesslogs/2016-01-01-00-00``. 

    {
        "name": "File",
        "type": "batchsink",
        "properties": {
            "path": "hdfs://mycluster.net:8020/etl/accesslogs",
            "suffix": "YYYY-MM-dd-HH-mm"
        }
    }

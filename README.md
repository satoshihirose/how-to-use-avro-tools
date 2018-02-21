# How to use avro-tools
## First of all, let's read specification of avro
* https://avro.apache.org/docs/1.8.1/spec.html

## Download avro-tools.jar
1.8.2 is the latest version for now.
* http://www.apache.org/dyn/closer.cgi/avro/
* http://ftp.kddilabs.jp/infosystems/apache/avro/avro-1.8.2/java/

## Show help
```
$ java -jar avro-tools-1.8.2.jar --help
Version 1.8.2
 of Apache Avro
Copyright 2010-2015 The Apache Software Foundation

This product includes software developed at
The Apache Software Foundation (http://www.apache.org/).
----------------
Available tools:
          cat  extracts samples from files
      compile  Generates Java code for the given schema.
       concat  Concatenates avro files without re-compressing.
   fragtojson  Renders a binary-encoded Avro datum as JSON.
     fromjson  Reads JSON records and writes an Avro data file.
     fromtext  Imports a text file into an avro data file.
      getmeta  Prints out the metadata of an Avro data file.
    getschema  Prints out schema of an Avro data file.
          idl  Generates a JSON schema from an Avro IDL file
 idl2schemata  Extract JSON schemata of the types from an Avro IDL file
       induce  Induce schema/protocol from Java class/interface via reflection.
   jsontofrag  Renders a JSON-encoded Avro datum as binary.
       random  Creates a file with randomly generated instances of a schema.
      recodec  Alters the codec of a data file.
       repair  Recovers data from a corrupt Avro Data file
  rpcprotocol  Output the protocol of a RPC service
   rpcreceive  Opens an RPC Server and listens for one message.
      rpcsend  Sends a single RPC message.
       tether  Run a tethered mapreduce job.
       tojson  Dumps an Avro data file as JSON, record per line or pretty.
       totext  Converts an Avro data file to a text file.
     totrevni  Converts an Avro data file to a Trevni file.
  trevni_meta  Dumps a Trevni file's metadata as JSON.
trevni_random  Create a Trevni file filled with random instances of a schema.
trevni_tojson  Dumps a Trevni file as JSON.
```

## Get metadata to see schema
```
$ java -jar avro-tools-1.8.2.jar getmeta ./run-1519182095115-part-r-00000
log4j:WARN No appenders could be found for logger (org.apache.hadoop.metrics2.lib.MutableMetricsFactory).
log4j:WARN Please initialize the log4j system properly.
log4j:WARN See http://logging.apache.org/log4j/1.2/faq.html#noconfig for more info.
avro.schema	{"type":"record","name":"DynamicRecord","namespace":"root","fields":[{"name":"col0","type":["long","null"]},{"name":"col1","type":["string","null"]}]}
```

## Change codec to snappy from null
supported codecs are null, deflate, snappy
```
$ java -jar avro-tools-1.8.2.jar recodec ./run-1519182095115-part-r-00000 --codec snappy >> ./run-1519182095115-part-r-00000.snappy
```

## Then, you can see avro.codec property
```
java -jar avro-tools-1.8.2.jar getmeta ./run-1519182095115-part-r-00000.snappy
log4j:WARN No appenders could be found for logger (org.apache.hadoop.metrics2.lib.MutableMetricsFactory).
log4j:WARN Please initialize the log4j system properly.
log4j:WARN See http://logging.apache.org/log4j/1.2/faq.html#noconfig for more info.
avro.schema	{"type":"record","name":"DynamicRecord","namespace":"root","fields":[{"name":"col0","type":["long","null"]},{"name":"col1","type":["string","null"]}]}
avro.codec	snappy
```

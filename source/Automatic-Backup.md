# Automatic Backup Plugin

Java class implementation:
```java
com.orientechnologies.orient.server.handler.OAutomaticBackup
```

# Introduction

Configures an automatic backup/export of databases. This task is configured as a [Server handler](DB-Server.md#handlers). The task can be configured in easy way by changing parameters:
* **enabled**: true to turn on, false (default) is turned off
* **mode**: one of the following values (Since v2.2):
 * **FULL_BACKUP** (default), executes a full backup. Before v2.2 this was the only available option. This operation is blocking
 * **INCREMENTAL_BACKUP**, executes an incremental backup. A directory per database is used. This operation is not blocking
 * **EXPORT**, executes an export of database in gzipped-json format . This operation is not blocking
* **exportOptions**: Options for the export mode (Since v2.2)
* **delay**: delay time. You can use different suffixes to specify different measures:
  * **ms** for milliseconds. Example 10000ms means 10 seconds
  * **s** for seconds. Example 10s means 10 seconds
  * **m** for minutes. Example 5m means 5 minutes
  * **h** for hours. Example 24h means every day
  * **d** for days. Example 1d means every day
* **firstTime** time of first backup schedule. It's referring at current day. The format is: `HH:mm:ss`. GMT timezone is used.
* **target.directory**: target directory, the default is "backup"
* **target.fileName**: target file name configurable using the following variables between <code>${}</code>:
  * **<code>${DBNAME}</code>**, as the database name
  * **<code>${DATE}</code>**, as the current date following the format. For the complete syntax look at [Java DateTime syntax](http://download.oracle.com/javase/1,5.0/docs/api/java/text/SimpleDateFormat.html)
* **db.include**: database list to include. If empty means all the databases
* **db.exclude**: database list to exclude
* **bufferSize**: In memory buffer size to use in compression. Default is 1MB. Bigger means faster backup but more RAM used (Since 1.7)
* **compressionLevel**: Compression level of the resulting ZIP file. Default is maximum: 9. Set it lower if backup takes too much time (Since 1.7)

Default configuration in orientdb-server-config.xml

```xml
<!-- AUTOMATIC BACKUP, TO TURN ON SET THE 'ENABLED' PARAMETER TO 'true' -->
<handler class="com.orientechnologies.orient.server.handler.OAutomaticBackup">
  <parameters>
    <parameter name="enabled" value="false" />
     <!-- CAN BE: FULL_BACKUP, INCREMENTAL_BACKUP, EXPORT -->
     <parameter name="mode" value="FULL_BACKUP"/>
     <!-- OPTION FOR EXPORT -->
     <parameter name="exportOptions" value=""/>
    <parameter name="delay" value="4h" />
    <parameter name="target.directory" value="backup" />
    <!-- ${DBNAME} AND ${DATE:} VARIABLES ARE SUPPORTED -->
    <parameter name="target.fileName" value="${DBNAME}-${DATE:yyyyMMddHHmmss}.zip" />
    <!-- DEFAULT: NO ONE, THAT MEANS ALL DATABASES. USE COMMA TO SEPARATE MULTIPLE DATABASE NAMES -->
    <parameter name="db.include" value="" />
    <!-- DEFAULT: NO ONE, THAT MEANS ALL DATABASES. USE COMMA TO SEPARATE MULTIPLE DATABASE NAMES -->
    <parameter name="db.exclude" value="" />
    <parameter name="compressionLevel" value="9"/>
    <parameter name="bufferSize" value="1048576"/>
  </parameters>
</handler>
```

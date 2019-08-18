[![Build Status](https://buildhive.cloudbees.com/job/tfredrich/job/DateAdapterJ/badge/icon)](https://buildhive.cloudbees.com/job/tfredrich/job/DateAdapterJ/)

DateAdapterJ is a small java library to support the conversion of java.util.Date instances
to and from String instance in commonly-utilized, standard date (and timestamp) formats.
This facilitates JSON and XML date and timestamp conversion, as well as, common web UI
parameter processing.   The goal is to easily support ISO 8601 calendar date and time
point formats, as well as, common UI representations right out of the box.

The primary reason DateAdapterJ exists is that it allows a list of supported input formats,
in priority order, and accepts all of them, parsing them into a java.util.Date instance,
always using UTC internally, so no conversions need to happen on the server side.

ISO8601 format, or more commonly known as "ISO format," facilitate cross-platform date
formats.  The ISO 8601 standard, however, describes both date formats and time formats, as
well as both combined, which it refers to as time point formats.  Please refer to the
standard here: 

ISO 8601 Format is not necessarily easily supported via Java's SimpleDateFormat (prior to 
Java 7).
Not all ISO8601 date and time formats are supported by DateAdapterJ.  Only ISO 8601
'Calendar Date' and 'Time Point' formats are supported.  Other formats (e.g. time formats
independent of date) are not.

RFC 1123, RFC 1036, and RFC 850 are also supported for full dates in the HTTP
specification.  This is useful for parsing and setting full dates in HTTP 1.0 and HTTP
1.1 headers (see HttpHeaderTimestampAdapter).

DateAdapterJ parses the following ISO 8601 Calendar Date formats (via DateAdapter):

1. YYYY-MM-DD
2. YYYYMMDD

In addition to the above date formats, DateAdapterJ supports the following date formats,
which are common on UIs in the United States:

4. MM/DD/YYYY
5. YYMMDD

The default date output format is:

* YYYY-MM-DD

For ISO 8601, times are of the form:

1. HH:MM:SS
2. HHMMSS

There are no time zone designators utilized in the standard.  Instead there is only local
time or UTC, with UTC being represented by a 'Z' at the end of the time point.  However,
ISO 8601 also makes use of time zone offsets which may be of the form:

1. +/-HH:MM
2. +/-HHMM
3. +/-HH

Thus, 10:30:45Z is the same time as 03:30:45-07:00 (Mountain time).  DateAdapterJ does
not currently parse times independent of a date.  ISO 8601 refers to dates combined with
times as time points. Currently supported ISO 8601 time point formats are
(via Iso8601TimepointAdapter):

1.  YYYY-MM-DDTHH:MM:SSZ
2.  YYYY-MM-DDTHH:MM:SS+HH:MM
3.  YYYY-MM-DDTHH:MM:SS+HHMM
4.  YYYY-MM-DDTHH:MM:SS+HH
5.  YYYY-MM-DDTHH:MMZ
6.  YYYY-MM-DDTHH:MM+HH:MM
7.  YYYY-MM-DDTHH:MM+HHMM
8.  YYYY-MM-DDTHH:MM+HH
9.  YYYY-MM-DDTHHMM+HH:MM
10. YYYY-MM-DDTHHMM+HHMM
11. YYYY-MM-DDTHHMM+HH

Additional time stamp formats are also parsed, where 'sss' represents milliseconds:

1. YYYY-MM-DDTHH:MM:SS.sssZ
2. YYYY-MM-DDTHH:MM:SS.sss+HH:MM
3. YYYY-MM-DDTHH:MM:SS.sss+HHMM
4. YYYY-MM-DDTHH:MM:SS.sss+HH

The default time point output format for TimestampAdapter() (if you want to use the ISO
8601 time point format, use new Iso8601TimepointAdapter) is:

* YYYY-MM-DDTHH:MM:SS.sssZ

The default time point output format for Iso8601TimepointAdapter is:

* YYYY-MM-DDTHH:MM:SSZ

Supported date and time point formats can easily be configured for your various
input/output processing needs.

RFC 1123, RFC 1036, RFC 850--Full dates in HTTP headers.  HttpHeaderTimestampAdapter()
supports the following formats for input (e.g. as incoming HTTP headers):

1. EEE, dd MMM yyyy HH:mm:ss z	//e.g. Sun, 06 Nov 1994 08:49:37 GMT ; RFC 822, updated by RFC 1123
2. EEEE, dd-MMM-yy HH:mm:ss z	//e.g. Sunday, 06-Nov-94 08:49:37 GMT ; RFC 850, obsoleted by RFC 1036
3. EEE MMM d HH:mm:ss yyyy		//e.g. Sun Nov  6 08:49:37 1994 ; ANSI C's asctime() format
4. YYYY-MM-DDTHH:MM:SS.sssZ		// Default time stamp output format from TimestampAdapter.
5. YYYY-MM-DD					// Default date output format from DateAdapter.

To support RFC1123, the default output format of HttpHeaderTimestampAdapter is:

* EEE, dd MMM yyyy HH:mm:ss GMT 

Note: All dates (and times) are converted to UTC internally.  So output is always UTC adjusted.

Maven Usage
===========
Stable:
```xml
		<dependency>
			<groupId>com.strategicgains</groupId>
			<artifactId>DateAdapterJ</artifactId>
			<version>1.1.4</version>
		</dependency>
```
Development:
```xml
		<dependency>
			<groupId>com.strategicgains</groupId>
			<artifactId>DateAdapterJ</artifactId>
			<version>1.1.5-SNAPSHOT</version>
		</dependency>
```
Or download the jar directly from: 
http://search.maven.org/#search%7Cga%7C1%7CDateAdapterJ

Note that to use the SNAPSHOT version, you must enable snapshots and a repository in your pom file as follows:
```xml
  <profiles>
    <profile>
       <id>allow-snapshots</id>
          <activation><activeByDefault>true</activeByDefault></activation>
       <repositories>
         <repository>
           <id>snapshots-repo</id>
           <url>https://oss.sonatype.org/content/repositories/snapshots</url>
           <releases><enabled>false</enabled></releases>
           <snapshots><enabled>true</enabled></snapshots>
         </repository>
       </repositories>
     </profile>
  </profiles>
```

Change History/Release Notes:

Release 1.1.5-SNAPSHOT (on Master)
==================================
* Uses 1.8 source and target.
* Added LocalDateAdapter to support use of LocalDate for date-only properties.

Release 1.1.4 - 28 Jul 2015
===========================
* Upgraded Java output to 1.7 source and target.

Release 1.1.3 - 12 Mar 2015
===================================================================================
* Added unit test for parsing dates of format: "2014-11-20T10:43:24+10:45"

Release 1.1.2 - 4 Mar 2013
===================================================================================
* Fixed issue with Iso8601TimepointAdapter not using timepoint output format.

Release 1.1.1 - 16 Jan 2013
===================================================================================
* Introduced Maven build, removing Ant build artifacts.

Release 1.1.0 - 16 Oct 2012
===================================================================================
* Released to Maven Central repository (08 Jan 2013).
* Introduced HttpHeaderTimestampAdapter to support RFC1123, RFC1036 and RFC850 for full dates in
  HTTP headers.  See HTTP 1.1 specification at http://www.w3.org/Protocols/rfc2616/rfc2616-sec3.html
  section 3.3.1 -- Full Date.
* Renamed Iso8601TimestampAdapter to Iso8601TimepointAdapter.

Release 1.0.0
===================================================================================
* Initial release

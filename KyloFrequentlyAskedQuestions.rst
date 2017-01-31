
=================================================
Frequently Asked Questions
=================================================

Is NiFi compatible with Cloudera, Hortonworks, Map R, EMR, and vanilla Hadoop distributions?
--------------------------------------------------------------------------------

Yes. Kylo generally relies on standard Hadoop APIs and common technologies like HDFS/S3, Hive, and Spark. NiFi operates on the "edge" so isn't bound to any particular
Hadoop distribution It is therefore compatible with most Hadoop distributions although we only provide install instructions for Cloudera and Hortonworks.

Does Kylo support Apache NiFi and Hortonworks DataFlow (HDF)? What is the difference?
--------------------------------------------------------------------------------------

Yes, Kylo support vanilla Apache NiFi or NiFi bundled with Hortonworks DataFlow. HDF bundles Apache NiFi, Storm, and Kafka within a distribution. Apache NiFi within HDF contains the same codebase
as the open-source project.


What is Kylo's value-add over plain Apache NiFi?
-------------------------------------------------------

NiFi acts as Kylo's orchestration engine and framework for data processing on the edge. It
doesn’t itself provide all the tooling required for a Data Lake
solution.

-  Write-once, use many times. NiFi is a powerful IT tool for designing
   pipelines but most Data Lake feeds utilize just a small number of
   unique flows or “patterns". Kylo allows IT the flexibility to
   design then register a NiFi template as the basis of feeds. This enables
   non-technical business users to configure dozens, or even hundreds of
   new feeds through our simple, guided stepper-UI. In other words, our
   UI allows users to setup feeds without having to code them in
   NiFi. As long as the basic ingestion pattern is the same, there is no
   need for new coding. Business users will be able to bring in new data
   sources, perform standard transformations, and publish to target
   systems.

-  Our Operations Dashboard is a superior UI for monitoring data feeds.
   It provides centralized health monitoring of feeds and Data Lake
   services, Service Level Agreements, data quality metrics reporting,
   and alerts.

-  Our toolset offers key Data Lake features such as metadata search,
   data discovery, wrangling, data browse, and event-based feed
   execution (to chain together flows).

-  Kylo adds a set of Data Lake specific NiFi extensions:

-  custom NiFi custom processors for operations such as: Data Profile,
   Data Cleanse, Data Validate, Merge/Dedupe, and Extract Table with
   high-water.

-  custom NiFi processors for utilizing Hadoop for processing.: Spark
   exec, Spark shell, Hive and Spark via JDBC/Thrift ,and others. These
   processors aren't yet available with vanilla NiFi

-  pre-built NiFi templates that implement Data Lake best practices:
   Data Ingest, ILM, and Data Processing

What license does Kylo include?
===========

Think Big/Teradata offers Kylo underthe Apache 2.0 license.


Is paid commercial support available for Kylo?
----------------------

Yes, Think Big offers support subscription at the standard and enterprise level. Please visit the Think Big Analytics website for more information.

How is Kylo differentiated against similar commercial products?
---------------------------------------------------------------

Kylo has similar capabilities to Podium and Zaloni Bedrock. Kylo is an open-source option. One differentiator is Kylo's extensibility. Kylo provides
plug-in architecture and makes powerful use of NiFi templates.

What does Kylo mean?
----------------------------------

Kylo is a play on the greek word meaning "flow".

Architecture
============

What is the deployment architecture? 
-------------------------------------

Kylo is a web application typically installed on a Linux “edge node” of the Hadoop
cluster. Kylo contains a number of special purposed code for data lake operations leveraging Spark
and Apache NiFi.

It uses open source to manage data pipelines called “feeds” configured in Kylo UI and executed
within Apache NiFi.


Metadata
========

What type of metadata does Kylo capture?
------------------------------------

Kylo captures all business and technical (for example, schema) metadata
defined during the creation of feeds and categories in addition to process lineage
as relationships between feeds, sources, and sinks. Kylo automatically capture all operational
metadata generated during a pipeline. In addition, Kylo stores job and feed
performance metadata and SLA metrics. We also generate data prsofile
statistics which act as metadata. We capture version metadata and feed
configuration changes.

How does Kylo support metadata exchange with 3rd party metadata servers
-------------------------------------------------------------------

Kylo's metadata server has REST APIs that could be used to do metadata
exchange fully documented in Swagger.

Often the actual question isn’t whether/how we support metadata
exchange, but how we would map our metadata model to the 3rd party
model. All of the metadata entities we have modeled so far are focused
around Kylo use cases.

What is the metadata server?
----------------------------

A key part of Kylo's architecture relies on the open-source JBoss ModeShape
framework, which allows for dynamic schemas. This gives the business the
ability to extend entities with business metadata, etc. 

-  Dynamic schemas - provide extensible features for extending schema
   towards custom business metadata in the field

-  Versioning - ability to track changes to metadata over time

-  Text Search - flexible searching metastore

Portability - can run on sql and nosql databases

    See: \ `*http://modeshape.jboss.org/* <http://modeshape.jboss.org/>`__

How extensible is Kylo metadata model?
--------------------------------------

Very extensible due our use of ModeShape (see above). The Kylo
application allows an administrator to define standard business metadata
fields that users will be prompted to enter when creating feeds and categories.
The configuration can be done so that all feeds in a particular category
collect the same type of business metadata. This is all UI-driven
configuration. Separately, the model allows for us to extend the data
model to capture other types of technical metadata or lineage
relationships outside the purview of Kylo.

Are there any business-related data captured, or are they all operational metadata?
-----------------------------------------------------------------------------------

Yes, see above. Business metadata fields can be defined by the customer
and will appear in the UI during the feed setup process.

What does the REST API look like?
---------------------------------

Please access the REST documentation through a running Kylo instance  http://kylo-host:8400/api-docs/index.html

Does Kylo provide a visual lineage?
-----------------------------------

Yes, Kylo provides a visual process lineage feature for exploring relationships between feeds and shared sources and sinks.  Job instance level lineage is stored as "steps" visible in the feed job
history.

What type of process metadata do we capture?
--------------------------------------------

We capture job and step level information on the status of the process,
with some information on the number of records loaded, how long it took,
when it was started and finished, and how many errors were generated. We
capture operational metadata at each step, which can include record
counts, etc., dependent on the type of step. We also capture job and
step status and exceptions, etc.

What type of data or record lineage?
------------------------------------

Kylo tracks lineage as relationships between feeds. A feed in Kylo
represents a significant unit movement of data between source(s) and
sink (for example an ingest, transformation pipeline, or export of data)
but it does not imply a particular technology since transformations can
occur in Spark, Hive, Pig, Shell scripts, or even 3rd party tools like
Informatica. We believe the feed lineage has advantages over bottom-up
approach other common tools provide. A feed
is enriched with business data, Service Level Agreements, job history,
and technical metadata about any sources and sinks it uses, as well as
operational metadata about datasets.

When tracing lineage, we are capable of providing a much more relatable
representation of dependencies (either forwards or backwards through the
chain) than can other tools.

Object lineage: ability to perform impact analysis on backward and
forward at object level (table level,attribute level).

Does Kylo track object-level lineage (table,attribute)?
-------------------------------------------------------

Kylo does not automatically capture metadata for each transform at the
lowest level, and does not currently perform impact analysis on table
structure changes.

Object lineage may be possible through tools such as Cloudera Navigator or
Atlas, which can be used as a supplement to Kylo. Keep in mind that
these tools have blind spots in that they are limited to certain
technologies like Hive or Impala. If a transform occurs in Spark, it
will not be able to trace it. These tools also do not perform automatic
impact analysis.

Why is direct lineage automatically tracked between feeds but not table objects?
--------------------------------------------------------------------------------

In a traditional EDW/RDBMS solution, a table is the de-facto storage
unit and SQL primitives (filter,join,union,etc.) can fully represent all
transforms. In Hadoop, we have to consider nontraditional concepts such
as streams, queues, NoSQL/HBase, flat files, external tables w/ HDFS,
spark/pig jobs, map-reduce, python, etc. Kylo is very flexible. NiFi has
150 existing connectors to these different technologies and transforms
where we often have no insight into the embedded process. We
specifically allow a designer to use all of these capabilities. The
downside is that there is no reliable mechanism for us to automatically
capture object-level lineage through all these potential sources/sinks
and processes that could come into play.

Atlas and Navigator ignore the reality above and only track transforms
between Hive/Impala tables via HQL. These two tools really only track
lineage for Hive transactions. This works just fine until you introduce
a source outside of Hive or an unsupported transformation technology
(for example, Spark, Pig) and now your lineage is broken! Furthermore,
it presents a very low-level and almost meaningless explanation of what
is going on unless you are a DBA. With Kylo, we want to provide
something more meaningful and reliable.

A feed in our metadata model is a 1st class entity representing a
meaningful movement of data. Feeds generally process data between
source(s) and sinks(s). An example would be an Ingest or a Wrangle job.
The internals of a feed can involve very complex steps. Our feed
abstraction makes those messy details a “black box”. The beauty of a
feed is it is an incredibly enriched object for communicating metadata:

-  Business metadata: Descriptions of feed purpose as well as any other
   business metadata specified by the creator.

-  Intra-feed lineage: All job executions, steps, and the operational
   metadata are captured including profile statistics. Note: operational
   metadata includes source files, counts, etc.

-  DAG: We can provide access to the full pipeline in human readable
   form (that is, NiFi flow).

-  Service Level Agreement: Its performance over time.

-  Technical metadata: Any tables created, its schema and validation and
   cleansing rules.

-  Finally and most importantly for lineage: A feed can declare a
   dependency on other feed(s). Currently this can be declared through
   our UI via the precondition capability. This dependency relationship
   can be n-deep and n-wide then queried (forward or backward) through
   the REST API. This allows us to understand lineage from the
   perspective of chains of feeds each with their associated treasure
   trove of meaningful metadata. 

Development Lifecycle
=====================

What's the development process using Kylo? 
-------------------------------------------

Pipelines developed with Apache NiFi can be developed in one environment
and then imported into UAT and production after testing. Once
the NiFi template is registered with Think Big’s system then a business
analyst can configure new feeds from it through our guided user
interface.

Alternatively an existing Kylo feed can be exported from an environment to a zip file which contains a combination of the underlying template and the metadata. The
package can then be imported in the production NiFi environment by an administrator.

Do we support approval process to move feeds into production?
-------------------------------------------------------------

Kylo generation using Apache NiFi does NOT require a restart to deploy
new pipelines. By locking down production NiFi access, users could be
restricted from creating new types of pipelines without a formal
approval process.

Cannew feeds be created in automated fashion instead of manually through the UI?
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

You could write scripts that use Kylo APIs to generate those feeds. See Swagger documentation (above).

Tool Comparisons
================

Is it similar to Cloudera Navigator, Apache Atlas
-------------------------------------------------

In some ways. Kylo is not trying to compete with these and could certainly
imagine integration with these tools. However, we also have an extensible
metadata server. Navigator is a governance tool that comes as part the
Cloudera Enterprise license. Among other features, it provides data
lineage of your Hive SQL queries. We think this is useful but only
provides part of the picture. Our framework is really the foundation of
an entire data lake solution. It captures both business
and operational metadata. It tracks lineage at the feed-level. Kylo provides IT Operations with a useful dashboard, ability to
track/enforce Service Level Agreements, and performance metrics.

How does it compare to traditional ETL tools like Talend, Informatica, Data Stage?
----------------------------------------------------------------------------------

Many ETL tools are focused on SQL transformations using their own
technology cluster. Hadoop is really ELT (extract and load raw data,
then transform). But typically the data warehouse style transformation
is into a relational schema such as a star or snowflake. In Hadoop it is
in another flat denormalized structure. So we don’t feel those expensive
and complicated technologies are really necessary for most ELT
requirements in Hadoop. Kylo provides a user interface for an end-user to
configure new data feeds including schema,security,validation, and
cleansing. Kylo provides the ability to wrangle and prepare
visual data transformations using Spark as an engine. W

Potentially Kylo can invoke traditional ETL tools, e.g. wrap 3rd party ETL jobs as "feeds" and so leverage these technologies.

Scheduler
=========

How to set job priority in Pipeline?
------------------------------------

Kylo exposes the ability to control which yarn queue a task executes on. Typically scheduling this is done through the scheduler. There are some
advanced techniques in NiFi that allow further prioritization for shared
resources. 

Can Pipeline support complicated ETL scheduling?
------------------------------------------------

We support the flexibility of cron-based scheduling, butr also
timer-based, or event-based using JMS and an internal Kylo ruleset. NiFi embeds the Quartz.

What’s the difference between “timer” and “cron” schedule strategies?
---------------------------------------------------------------------

Timer is fixed interval, “every 5 min or 10 seconds”. Cron can be
configured to do that as well but can handle more complex cases like
“every tues at 8AM and 4PM”.

Do we support message-trigger schedule strategy
-----------------------------------------------

Yes, typically JMS or HTTP-based.

Does Kylo support chaining feeds? One data feed consumed by another data feed.
----------------------------------------------------------------------------------

Kylo supports event-based triggering of feeds based on preconditions or rules. One can define rules in the UI that determine when to run a
feed such as “run when data has been processed by feed a and feed b and
wait up to an hour before running anyway”. We support simple rules up to
very complicated rules requiring use of our API.

Security
========

Does Kylo have roles, users and privileges management function?
-------------------------------------------------------------------

Kylo uses Spring Security. It can integrate with Active Directory, Kerberos, LDAP,
or most any authentication provider.

Kylo supports the definition of roles (or groups) and the specific permissions a user with that role can perform down to the function level.

Detailed Questions
==================

How does “incremental” loading strategy of a data feed work?
------------------------------------------------------------

Kylo supports a simple incremental extract component. We maintain a
high-water mark for each load using a date field in the source record.
We can further configure a backoff or overlap to ensure that we don’t
miss records.


When we create a data feed for a relational database, how is the source database’s schema affected?
---------------------------------------------------------------------------------------------------

Kylo inspects the source schema and exposes it through our user
interface for the user to be able to configure feeds.

What kinds of database can be supported in Kylo?
---------------------------------------------------------------------------------------------------------

We store metadata and job history in MySQL or Postgres. For sourcing
data, we can theoretically support any database that provides a JDBC
driver. It has been tested with Teradata, SQL Server, Oracle, Postgres, and MySQL.

When we choose record format as “delimited”, how to handle the data of columns that contain characters the same as “delimiter character”?
-----------------------------------------------------------------------------------------------------------------------------------------

You can easily configure options for the text SERDE, which allows you to define escape characters.

Does Kylo support creating Hive table automatically after the source data is put into Hadoop?
-------------------------------------------------------------------------------------------------

We have a stepper “wizard” that is used to configure feeds and can
define a table schema in Hive. The stepper infers the schema looking at
a sample file or from the database source. It automatically creates the
Hive table on the first run of the feed.


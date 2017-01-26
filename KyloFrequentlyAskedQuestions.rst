
=================================================
Frequently Asked Questions
=================================================

When will this asset be generally available for field use?
----------------------------------------------------------

We are actively using Kylo with multiple customers internationally and
in the Americas. We are currently limiting use to Think Big projects
only, until the open source release. We expect it will have a stable 1.0
release in the Q1 2017 timeframe.

Is NiFi compatible with Cloudera, Map R, Hortonworks, and vanilla distributions?
--------------------------------------------------------------------------------

Yes! NiFi operates on the "edge" and isn't bound to any particular
Hadoop distribution It is therefore compatible
with \ **any** distribution we choose to support. 

How do we position NiFi against Hortonworks DataFlow? 
------------------------------------------------------

HDF appears to be simply the Hortonworks marketing of open-source Apache
NiFi distribution. We are watching closely to see if the two
technologies diverge or if Hortonworks starts holding back contributions
to the Apache NiFi core. There has been no evidence of this 6 months
after acquisition.

The good news is that we should be able to use DataFlow when working
with Hortonworks and vanilla Apache NiFi when working with CDH and other
distributions. The difference may matter to clients from a support
contract perspective. If a client has a Hortonworks contract, they would
probably use DataFlow. (Note: HDF is an add-on for support).

The number of enhancements produced by the Apache NiFi community has
gone up significantly since Hortonworks acquired. There were about 50
pre-built generic processors when we started looking at NiFi in
late-Dec. They have more than doubled in four months. NiFi went from
v0.3.1 to v0.6.1 in the same timeframe. Most of the changes appear to be
extensions to the peripherals (like processors), not the core engine
itself which appears to be very solid.

What is Think Big's value-add over plain NiFi/DataFlow?
-------------------------------------------------------

NiFi is similar to Spring Batch as a technology, specifically an
orchestration engine and framework for data processing on the edge. It
doesn’t itself provide all the tooling required for a Data Lake
solution.

Here are some talking points:

-  Write-once, use many times. NiFi is a powerful IT tool for designing
   pipelines but most Data Lake feeds utilize just a small number of
   unique flows or “patterns". Think Big allows IT the flexibility to
   design then register a NiFi template with our tool. This enables
   non-technical business users to configure dozens, or even hundreds of
   new feeds through our simple, guided stepper-UI. In other words, our
   UI allows users to set up pipeline without having to code them in
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

-  Think Big additionally accelerates NiFi development through our NiFi
   extensions:

-  custom NiFi custom processors for operations such as: Data Profile,
   Data Cleanse, Data Validate, Merge/Dedupe, and Extract Table with
   high-water.

-  custom NiFi processors for utilizing Hadoop for processing.: Spark
   exec, Spark shell, Hive and Spark via JDBC/Thrift ,and others. These
   processors aren't yet available with vanilla NiFi

-  pre-built NiFi templates that implement Data Lake best practices:
   Data Ingest, ILM, and Data Processing

Open source
===========

What can ThinkBig/Teradata employees share with customers and partners today?
-----------------------------------------------------------------------------

Think Big plans to release as open source under the Apache 2.0 license
in the Feb 2017 timeframe. Customers given the source code prior to
release will be asked to sign an NDA restricting them from publicly
releasing the source until after our formal release.

Why are we waiting so long to release?
--------------------------------------

The rationale for announcement later this year is to have production
experience, customer testimonials, and case studies (some may be
anonymous), as well as to give the R&D team time to package and document
(we are currently focused on making the first customers successful). It
also would let us line up some partners for support, for example,
Cloudera, Alation, DataBricks, Podium. We would like to do this
correctly with press release, fanfare, etc.

Will we still attempt to charge an up-front fee for this IP on new projects?
----------------------------------------------------------------------------

No. However we will offer commercial support for the framework as a
standard option that we encourage customers to buy.

How will support work?
----------------------

We will be developing a Support Subscription model similar to other open
source distributors such as Hortonworks, etc. We will be targeting an
experimental annual price $100K for up to 200 nodes.

How are we positioning this framework vs. products like Podium?
---------------------------------------------------------------

The framework is intended to be complimentary to both open source and
product-based implementations including Podium. We have used it
successfully in both modes in the past. Naturally, there is some
significant overlap in capabilities and we’ll work with project teams to
help them use the right capabilities.

So what should we call this thing?
----------------------------------

Kylo is the name of the technology, or Velocity Engineering Framework,
powered by Kylo.

Architecture
============

What is the deployment architecture? 
-------------------------------------

Kylo is typically installed on a Linux “edge node” of the Hadoop
cluster, or in some cases as a “buffer server” at distributed locations
on the customer’s network to push data to HDFS.

It uses open source to manage data pipelines that we call “feeds”. The
legacy version supports Spring Batch. Kylo supports Apache NiFi. 

Kylo requires Postgres or MySQL. It requires Java 8 or later. It has
been tested on Cloudera or Hortonworks Hadoop distributions. It is
installed via RPM but can be manually installed as well.
See \ *`Deployment Architecture
diagram <https://wiki.thinkbiganalytics.com/display/RD/Data+Lake+Accelerator+Architecture+Diagram>`__.*

Is pipeline compatible with "Data Lake architecture"? for example Landing Zone, Suspense Zone, etc.
---------------------------------------------------------------------------------------------------

It is compatible in theory, but pipeline doesn’t use most of those
terms; of course pipelines can be configured to do so, either confined
to a zone or integrated to zones.

Do we have diagrams to illustrate the internal information flow, controlling flow and data flow of whole system?
----------------------------------------------------------------------------------------------------------------

See the slide deck and send follow-up questions if not
answered. \ `*Kylo - Next Generation
(Kylo)* <https://wiki.thinkbiganalytics.com/display/RD/Kylo>`__

What are the extensible aspects of the framework? What customization methods are available to developers?
---------------------------------------------------------------------------------------------------------

See slide
deck \ *`here <https://wiki.thinkbiganalytics.com/download/attachments/11305103/Extension%20Points.pptx?version=1&modificationDate=1469050064000&api=v2>`__.*

Metadata
========

What type of metadata do we capture?
------------------------------------

We capture all business and technical (for example, schema) metadata
defined during the creation of feeds and categories. We capture lineage
as relationships between feeds. We automatically capture all operational
metadata generated during a pipeline. We capture job and feed
performance metadata and SLA metrics. We also generate data profile
statistics which act as metadata. We capture version metadata and feed
configuration changes.

How do we support metadata exchange with 3rd party metadata servers
-------------------------------------------------------------------

Kylo's metadata server has REST APIs that could be used to do metadata
exchange. We don’t have a single API call to export all, so this would
need to be written in the integration layer or through a new API written
by R&D. See \ *`Metadata REST
API <https://wiki.thinkbiganalytics.com/download/attachments/11305103/Metadata%20REST%20API.pptx?version=1&modificationDate=1469049131000&api=v2>`__.*
Often the actual question isn’t whether/how we support metadata
exchange, but how we would map our metadata model to the 3rd party
model. All of the metadata entities we have modeled so far are focused
around Kylo use cases. Some work by R&D will be needed, not only to
support your integration use case, but also in figuring out how to map
similar concepts. 

Can we import business glossary terms into Kylo?
------------------------------------------------

This would not be easy today although our team can give it some thought.
We are actively working on making the entire schema discovery mechanism
a pluggable component so we can support any future data formats that
come along as a plug-in. This also includes the ability to supply a
schema and the business glossary as a definition file during feed
creation. This is similar to how Podium works. The advantage of this
approach is that it can leverage existing metadata.

What is the metadata server?
----------------------------

A key part of our architecture relies on the open-source JBoss ModeShape
framework, which allows for dynamic schemas. This gives the business the
ability to extend entities with business metadata, etc. 

-  Dynamic schemas - provide extensible features for extending schema
   towards custom business metadata in the field

-  Versioning - ability to track changes to metadata over time

Text Search - flexible searching metastore

Portability - can run on sql and nosql databases

    See: \ `*http://modeshape.jboss.org/* <http://modeshape.jboss.org/>`__

How extensible is Kylo metadata model?
--------------------------------------

Very extensible due our use of ModeShape (see above). The Kylo
application allows an administrator to define standard business metadata
that users will be prompted to enter when creating feeds and categories.
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

See \ *`Metadata REST
API <https://wiki.thinkbiganalytics.com/download/attachments/11305103/Metadata%20REST%20API.pptx?version=1&modificationDate=1469049131000&api=v2>`__.*

Does Kylo provide a visual lineage?
-----------------------------------

Yes. The Kylo metadata server has REST APIs that could allow a pipeline
designer to supplement our lineage with additional metadata to provide a
much finer-grained capability. Additionally, REST APIs can be used to
record metadata that originated in 3rd party tools such as Informatica
for a complete picture.

What type of process metadata do we capture?
--------------------------------------------

We capture job and step level information on the status of the process,
with some information on the number of records loaded, how long it took,
when it was started and finished, and how many errors were generated. We
capture operational metadata at each step, which can include record
counts, etc., dependent on the type of step. We also capture job and
step status, start, stop, and exceptions, etc.

What type of data or record lineage?
------------------------------------

Kylo tracks lineage as relationships between feeds. A feed in Kylo
represents a significant unit movement of data between source(s) and
sink (for example an ingest, transformation pipeline, or export of data)
but it does not imply a particular technology since transformations can
occur in Spark, Hive, Pig, Shell scripts, or even 3rd party tools like
Informatica. We believe the feed lineage has advantages over bottom-up
approach tools like Cloudera Navigator (object lineage) provide. A feed
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

Object lineage is possible through tools such as Cloudera Navigator or
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

Is there a way to start from a table object and understand its lineage?
-----------------------------------------------------------------------

Yes, if a table is created by a feed, it is possible to navigate from a
table to its parent feed to dependent feed(s) to their associated table.
The metadata relationship is:

    1. Feed\_B explicitly has a dependency on Feed\_A. Navigate:Feed\_A <- (depends) Feed\_B

    2. Feed\_A writes to Table\_A, Feed\_B writes to Table\_B. Navigate: Feed\_A (sink:Table\_A) <- (depends) Feed\_B (sink:Table\_B)

Can we capture enhanced lineage using our metadata model if customer really wants a more explicit relationship between sources/sinks/processes?
-----------------------------------------------------------------------------------------------------------------------------------------------

Yes, this is possible using the REST API. The way to do this rests with
the designer role. The designer can use his own deep knowledge to create
a NiFi model that explicitly updates the metadata repository to create
detailed relationships. It involves extra up-front effort, but it
provides total flexibility. R&D can provide examples of using REST API
for this. See the
following \ `*example* <https://wiki.thinkbiganalytics.com/download/attachments/11305103/Table%20lineage%20REST%20example.docx?version=1&modificationDate=1472501750000&api=v2>`__.

This includes using our REST API to document external processes. For
example, transforms and flows outside of Kylo's purview (for example,
Informatica, bteq, talend).

Development Lifecycle
=====================

What's the development process using Kylo? 
-------------------------------------------

Pipelines developed with Apache NiFi can be developed in one environment
and then imported into UAT and production after testing. Thus the
production NiFi environment would be limited to an administrator. Once
the NiFi template is registered with Think Big’s system then a business
analyst can configure new feeds from it through our guided user
interface. We don’t see that as a development step. 

Do we support approval process to move feeds into production?
-------------------------------------------------------------

Kylo generation using Apache NiFi does NOT require a restart to deploy
new pipelines. By locking down production NiFi access, users could be
restricted from creating new types of pipelines without a formal
approval process. The Kylo user interface does not yet support
authorization and roles.

Suppose our clients have over 100 source systems and have over 10 thousand tables should be ingested into Hadoop, how to configure data feeds for them in Pipeline? One by one?
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

You could theoretically write scripts that use our APIs to generate
those feeds. We don’t have a utility to do it. One of the R&D engineers
has done something like that already, so we do know that it can be done.

Tool Comparisons
================

Is it similar to Podium?
------------------------

Podium is a product with some similar capabilities. It has some
capabilities that overlap with Talend and Trifacta. We build on Apache
NiFi and Spark, which is much more versatile in its support for data
movement. `*See comparison with
Podium.* <https://wiki.thinkbiganalytics.com/download/attachments/11305103/Kylo-Podium%20Comparison%20Dec-2016.pdf?version=1&modificationDate=1481737814000&api=v2>`__

-  We are open source, Apache 2.0 license.

-  Modernized architecture. We use Spark (vs. Apache Pig used by Podium)
   and Apache NiFi, which provides a much wider range of potential
   capabilities (for example streaming, ILM, custom templates). Podium
   provides a fixed ingest process that cannot be modified outside of
   their parameters. Our custom templating capability is a huge
   differentiator in terms of our ability to rapidly deploy tailor-fit
   solutions for our customers.

-  Podium’s operations features
   (dashboard/scheduling,monitoring/alerts,SLAs) are non-existent. They
   use Kylo for this when Think Big and Podium are used together.

-  Fully backed and influenced by Think Big’s deep experience (6 years,
   150+ clients) and all of our best practices in building solutions

Is it similar to Cloudera Navigator, Apache Atlas
-------------------------------------------------

In some ways. We aren't trying to compete with these and could certainly
imagine integration with them. However, we also have an extensible
metadata server. Navigator is a governance tool that comes as part the
Cloudera Enterprise license. Among other features, it provides data
lineage of your Hive SQL queries. We think this is useful but only
provides part of the picture. Our framework is really the foundation of
an entire solution, but in terms of metadata. It captures both business
and operational metadata. It tracks lineage at the feed-level (much more
useful). It provides IT Operations with a useful dashboard, ability to
track/enforce Service Level Agreements, and performance metrics.

How does it compare to traditional ETL tools like Talend, Informatica, Data Stage?
----------------------------------------------------------------------------------

Many ETL tools are focused on SQL transformations using their own
technology cluster. Hadoop is really ELT (extract and load raw data,
then transform). But typically the data warehouse style transformation
is into a relational schema such as a star or snowflake. In Hadoop it is
in another flat denormalized structure. So we don’t feel those expensive
and complicated technologies are really necessary for most ELT
requirements in Hadoop. VF provides a user interface for an end-user to
configure new data feeds including schema,security,validation, and
cleansing. VF provides the ability to perform complex Trifacta-like
visual data transformations using Spark as an engine. We could support
any transformation method that Hadoop supports. Potentially, we could
wrap Talend or ETL jobs as "feeds" and so leverage these technologies.

 

**From Douglas Moore:**

I just finished a project with Talend, and I’m starting one with Kylo.
I’ve also been on hands on client evals of Podium and Datameer.

My thoughts are:

-  Talend, Informatica, Pentaho, NiFi etc. are in the 2nd generation DI tools.

-  Podium and Kylo are in the 3rd generation tools data ingestion frameworks specifically designed for Hadoop Data Lakes.

   **2nd generation tools:**

   -  Build any to any mappings, across all platforms, perform almost any mapping you can dream of. The tool is a new language.

   -  Support Row operations and push down set operations.

   -  Set operations is where you get scale in data wrangling, this requires push down capabilities and learning SQL, Hive or Spark or Pig (depending on the tool).

   -  Support easy to modest big data operations, some advanced capabilities are missing from the UI (for example MR driver configuration, partitioners…)

   -  95% of your effort will be in designing and building the best practices (something 3rd generation gives you out of the box)

   -  Scaling out the # of feeds is typified by copy/paste rather than configure.

   -  2nd Generation tools require data analysts to write specs for ETL engineers to code, thus longer time to market.

   -  ETL engineers need to learn the 2nd generation tool AND Hive/Spark/Hadoop vagaries for push down, twice the skills are required.

   -  2nd generation tools require advanced architects to design the flows rather than getting a standard one out of the box.

   -  Configuration hell with matching tool versions and Hadoop versions and certifications.

   -  Higher CapEx, Human and OpEx costs



   **3rd generation tools:**

   -  Standardize the ingestion of structured data sources, standardize the error handling, field standardization, field edit checks.

   -  Tuned specifically for Hadoop Data Lake ingestion, and work well at this task.

   -  Provide add on data wrangling capabilities at scale using Pig or Spark.

   -  3rd generation tools provide a standard template. (Kylo’s is very easy to adjust and modify). These tools enable data analysts that know the data to ingest data without writing a spec for ETL engineers.

   -  Better in tune with self service capabilities.

How does it compare with Teradata Listener?
-------------------------------------------

Positioning:

-  Listener is a technology for self-service data ingest. Listener
   simplifies end-user (such as the application developer or marketing
   intelligence) and IT complexity by providing a single platform to
   deploy and manage an end-user specified ingestion and distribution
   model, significantly reducing deployment time and cost of ownership.

-  Kylo is a solutions framework for delivering Data Lakes on Hadoop and
   Spark. It performs ELT, with UI modules for IT Operations, Data
   Analysts, and Data Scientists.

Potentially complimentary capabilities:

-  Listener could be used as a tool and scalable ingest platform for the
   entire enterprise to stream data from real-time data sources into
   HDFS and HBase, along with TD and Aster.

-  Kylo can be used as the Data Lake application, and perform subsequent
   downstream transformations such as data processing, analytics, and
   export of data.

Features that overlap:

-  Kylo also allows users to do self-service ingest through a web-based
   interface.

-  Kylo relies on Apache NiFi connectors, which has some similar
   capabilities to Listener.

Scalability
===========

Do we have stress testing data for this solution? 
--------------------------------------------------

We did. See the slide deck. In the appendix there are a bunch of stress
test results. Basically, we offload much of processing to Hadoop so we
can scale quite high on the edge node. But the edge node can eventually
become a bottleneck, in which case we can scale out the edge node using
various techniques, including creating a small Apache NiFi cluster on
multiple edge nodes.

See Jeremy’s NiFi load tests results here:

   `*https://wiki.thinkbiganalytics.com/download/attachments/11305060/Data%20Lake%20Platform%20-%20International.pptx?version=1&modificationDate=1459753140000&api=v2* <https://wiki.thinkbiganalytics.com/download/attachments/11305060/Data%20Lake%20Platform%20-%20International.pptx?version=1&modificationDate=1459753140000&api=v2>`__

See slides 59-67

Also see \ `*Kylo benchmark
results* <https://wiki.thinkbiganalytics.com/display/RD/Kylo+Benchmark>`__

As part of a POC, Jeremy setup NiFi and Kylo on an edge node with a
small Hadoop/Spark cluster to characterize our solution and NiFi under
load . He was able to push our simple NiFi edge node to 500+ jobs. Like
Podium we do most of our actual data processing in Hadoop and Spark. A
single edge node simply moving data to HDFS can quickly become a
bottleneck, especially IO. We can scale by either reducing the IO
through the edge (as in buffer server approach of moving data to S3, or
HDFS bypassing a single edge) or through a small NiFi cluster across as
many edge nodes as needed.

How do we do capacity planning for a Kylo/NiFi cluster?
-------------------------------------------------------

There is no way to answer this without understanding the customer’s throughput and the processing required on the edge. I would generally start off by trying to get away with a single edge node and by following all of the best practices outlined in the videos under Admins/Architects – Clustering NiFi:

  *Kylo - Next Generation
  (Kylo)* <https://wiki.thinkbiganalytics.com/display/RD/Kylo>`__

If you need to scale up, then NiFi clustering (see video) provides a solution. If you understand throughput and potential for processing on the edge, where an edge node may become an IO bottleneck, then you could determine where you would need to scale.

Scheduler
=========

How to set job priority in Pipeline?
------------------------------------

Typically scheduling this is done through the scheduler. There are some
advanced techniques in NiFi that allow further prioritization for shared
resources. 

Can Pipeline support complicated ETL scheduling?
------------------------------------------------

We support the flexibility of cron-based scheduling, butr also
timer-based, or event-based using rules. See Quartz for more
information.

What’s the difference between “timer” and “cron” schedule strategies?
---------------------------------------------------------------------

Timer is fixed interval, “every 5 min or 10 seconds”. Cron can be
configured to do that as well but can handle more complex cases like
“every tues at 8AM and 4PM”.

Do we support message-trigger schedule strategy
-----------------------------------------------

Yes, we can absolutely support that. That is merely a modification to
our generic ingest template.

Does pipeline support chaining feeds? One data feed consumed by another data feed.
----------------------------------------------------------------------------------

Yes, this is covered in the slide deck. We support event-based
triggering of feeds. You can define rules that determine when to run a
feed such as “run when data has been processed by feed a and feed b and
wait up to an hour before running anyway”. We support simple rules up to
very complicated rules requiring use of our API.

Security
========

Does Pipeline have roles, users and privileges management function?
-------------------------------------------------------------------

Kylo uses Spring Security. It can integrate with Active Directory, LDAP,
or most any authentication provider.

The Operations Dashboard does not currently support roles as it is
typically oriented to a single role (IT Operations). Authorization could
be added.

Detailed Questions
==================

How does “incremental” loading strategy of a data feed work?
------------------------------------------------------------

Kylo supports a simple incremental extract component. We maintain a
high-water mark for each load using a date field in the source record.
We can further configure a backoff or overlap to ensure that we don’t
miss records. There is no CDC tool integration with Kylo. This would be
an exercise for a field engineering effort.

When we create a data feed for a relational database, how is the source database’s schema affected?
---------------------------------------------------------------------------------------------------

Kylo inspects the source schema and exposes it through our user
interface for the user to be able to configure feeds. This feature is
not available in the current generation.

What kinds of database can be supported in Pipeline? Please list all databases that pipeline can support.
---------------------------------------------------------------------------------------------------------

We store metadata and job history in MySQL or Postgres. For sourcing
data, we can theoretically support any database that provides a JDBC
driver. 

What drivers or tools internally used for a data feed from extracting data to putting into Hadoop HDFS?
-------------------------------------------------------------------------------------------------------

+-------------------------+--------------------------------------------------------------------------------------------+
| **Stage or Function**   | **Drivers or tools be used**                                                               |
+-------------------------+--------------------------------------------------------------------------------------------+
| Data Extracting         | JDBC, or data dumps to filesystem                                                          |
+-------------------------+--------------------------------------------------------------------------------------------+
| Data Compression        | ORC with SNAPPY is default. Archival supports a half dozen other compression techniques.   |
+-------------------------+--------------------------------------------------------------------------------------------+
| Job Scheduling          | Quartz engine by default                                                                   |
+-------------------------+--------------------------------------------------------------------------------------------+

Keep in mind that Kylo is designed to be an accelerator and not a
full-featured product. It can be customized as needed to suit the
client’s needs.

When we choose record format as “delimited”, how to handle the data of columns that contain characters the same as “delimiter character”?
-----------------------------------------------------------------------------------------------------------------------------------------

You can configure a SERDE, which allows you to define escape characters.

Does pipeline support creating Hive table automatically after the source data is put into Hadoop?
-------------------------------------------------------------------------------------------------

We have a stepper “wizard” that is used to configure feeds and can
define a table schema in Hive. The stepper infers the schema looking at
a sample file or from the database source. It automatically creates the
Hive table and the first run of the feed.

Is Apache NiFi integrated with Kylo?
------------------------------------

Yes, see slide deck and demo at \ `*Kylo - Next Generation
(Kylo)* <https://wiki.thinkbiganalytics.com/display/RD/Kylo>`__

Where is the pipeline configuration data stored? In database/file system?
-------------------------------------------------------------------------

We provide a user interface to configure pipelines or “feeds”. The
metadata is stored in a metadata server backed by MySQL (alternatively
Postgres).

How do you rerun a feed? What are the steps to restore original state before data ingest?
-----------------------------------------------------------------------------------------

One exciting feature is the ability of NiFi to replay a failed step.
This could be particularly useful for secondary steps of a pipeline. For
example, a flow successfully processes data into Hive, but fails to
archive into S3. We might be able to re-execute the S3 portion without a
full re-execution of the data. In general, we strive for idempotent
behavior so any step and data can be re-processed without duplication.

.. |image0| image:: media/common/thinkbig-logo.png
   :width: 3.09891in
   :height: 2.03724in

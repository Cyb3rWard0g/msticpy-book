# Package Structure

Security Tools Sub-package - *sectools*
---------------------------------------

This subpackage contains several modules helpful for working on security
investigations and hunting:

### auditdextract

:py`msticpy.sectools.auditdextract`{.interpreted-text role="mod"}

Module to load and decode Linux audit logs. It collapses messages
sharing the same message ID into single events, decodes hex-encoded data
fields and performs some event-specific formatting and normalization
(e.g. for process start events it will re-assemble the process command
line arguments into a single string).

### base64unpack

:py`msticpy.sectools.base64unpack`{.interpreted-text role="mod"}

Base64 and archive (gz, zip, tar) extractor. Input can either be a
single string or a specified column of a pandas dataframe. It will try
to identify any base64 encoded strings and decode them. If the result
looks like one of the supported archive types it will unpack the
contents. The results of each decode/unpack are rechecked for further
base64 content and will recurse down up to 20 levels (default can be
overridden). Output is to a decoded string (for single string input) or
a DataFrame (for dataframe input).

See `../data_analysis/Base64Unpack`{.interpreted-text role="doc"}

Sample notebook - [Base64Unpack Usage
Notebook](https://github.com/microsoft/msticpy/blob/master/docs/notebooks/Base64Unpack.ipynb)

### iocextract

:py`msticpy.sectools.iocextract`{.interpreted-text role="mod"}

Uses a set of builtin regular expressions to look for Indicator of
Compromise (IoC) patterns. Input can be a single string or a pandas
dataframe with one or more columns specified as input.

The following types are built-in:

-   IPv4 and IPv6
-   URL
-   DNS domain
-   Hashes (MD5, SHA1, SHA256)
-   Windows file paths
-   Linux file paths (this is kind of noisy because a legal linux file
    path can have almost any character) You can modify or add to the
    regular expressions used at runtime.

Output is a dictionary of matches (for single string input) or a
DataFrame (for dataframe input).

See `../data_analysis/IoCExtract`{.interpreted-text role="doc"}

Sample notebook - [IoCExtract Usage
Notebook](https://github.com/microsoft/msticpy/blob/master/docs/notebooks/IoCExtract.ipynb)

### tiproviders

:py`msticpy.sectools.tilookup`{.interpreted-text role="mod"}

The TILookup class can lookup IoCs across multiple TI providers. builtin
providers include AlienVault OTX, IBM XForce, VirusTotal and Azure
Sentinel.

The input can be a single IoC observable or a pandas DataFrame
containing multiple observables. Depending on the provider, you may
require an account and an API key. Some providers also enforce
throttling (especially for free tiers), which might affect performing
bulk lookups.

See `../data_acquisition/TIProviders`{.interpreted-text role="doc"}

Sample notebook - [TILookup Usage
Notebook](https://github.com/microsoft/msticpy/blob/master/docs/notebooks/TIProviders.ipynb)

### vtlookup

:py`msticpy.sectools.vtlookup`{.interpreted-text role="mod"}

Wrapper class around [Virus Total
API](https://www.virustotal.com/en/documentation/public-api/). Input can
be a single IoC observable or a pandas DataFrame containing multiple
observables. Processing requires a Virus Total account and API key and
processing performance is limited to the number of requests per minute
for the account type that you have. Support IoC Types:

-   Filehash
-   URL
-   DNS Domain
-   IPv4 Address

Sample notebook - [VTLookup Usage
Notebook](https://github.com/microsoft/msticpy/blob/master/docs/notebooks/VirusTotalLookup.ipynb)

### geoip

:py`msticpy.sectools.geoip`{.interpreted-text role="mod"}

Geographic location lookup for IP addresses. This module has two classes
for different services:

-   GeoLiteLookup - Maxmind Geolite (see <https://www.maxmind.com>)
-   IPStackLookup - IPStack (see <https://ipstack.com>) Both services
    offer a free tier for non-commercial use. However, a paid tier will
    normally get you more accuracy, more detail and a higher throughput
    rate. Maxmind geolite uses a downloadable database, while IPStack is
    an online lookup (API key required).

See `../data_acquisition/GeoIPLookups`{.interpreted-text role="doc"}

Sample notebook - [GeoIP Lookup Usage
Notebook](https://github.com/microsoft/msticpy/blob/master/docs/notebooks/GeoIPLookups.ipynb)

### eventcluster

:py`msticpy.sectools.eventcluster`{.interpreted-text role="mod"}

This module is intended to be used to summarize large numbers of events
into clusters of different patterns. High volume repeating events can
often make it difficult to see unique and interesting items.

The module contains functions to generate clusterable features from
string data. For example, an administration command that does some
maintenance on thousands of servers with a commandline such as:
`install-update -hostname {host.fqdn} -tmp:/tmp/{GUID}/rollback` can be
collapsed into a single cluster pattern by ignoring the character values
in the string and using delimiters or tokens to group the values.

This is an unsupervised learning module implemented using SciKit Learn
DBScan.

See `../data_analysis/EventClustering`{.interpreted-text role="doc"}

Sample notebook - [Event Clustering
Notebook](https://github.com/microsoft/msticpy/blob/master/docs/notebooks/EventClustering.ipynb)

### outliers

:py`msticpy.sectools.outliers`{.interpreted-text role="mod"}

Similar to the eventcluster module but a little bit more experimental
(read \'less tested\'). It uses SkLearn Isolation Forest to identify
outlier events in a single data set or using one data set as training
data and another on which to predict outliers.

### syslog\_utils

:py`msticpy.sectools.syslog_utils`{.interpreted-text role="mod"}

Module to support the investigation of Linux hosts through Syslog.
Includes functions to create host records, cluster logon events, and
identify user sessions containing suspicious activity.

### cmd\_line

:py`msticpy.sectools.cmd_line`{.interpreted-text role="mod"}

Module to investigation of command line activity. Allows for the
detection of known malicious commands as well as suspicious patterns of
behaviour.

### domain\_utils

:py`msticpy.sectools.domain_utils`{.interpreted-text role="mod"}

Module to support investigation of domain names and URLs with functions
to validate a domain name and screenshot a URL.

Notebook tools sub-package - *nbtools*
--------------------------------------

This is a collection of display and utility modules designed to make
working with security data in Jupyter notebooks quicker and easier.

See `../Visualization`{.interpreted-text role="doc"}

### Notebook widgets

:py`msticpy.nbtools.nbwidgets`{.interpreted-text role="mod"}

Common functionality such as list pickers, time boundary settings,
saving and retrieving environment variables into a single line callable
command.

See `../visualization/NotebookWidgets`{.interpreted-text role="doc"}

Sample notebook - [Event Clustering
Notebook](https://github.com/microsoft/msticpy/blob/master/docs/notebooks/NotebookWidgets.ipynb)

### Display functions

:py`msticpy.nbtools.nbdisplay`{.interpreted-text role="mod"}

Common display of things like alerts, events in a slightly more
consumable way than print()

### Process tree

:py`msticpy.nbtools.process_tree`{.interpreted-text role="mod"} -
process tree visualization.

See `../visualization/ProcessTree`{.interpreted-text role="doc"}

Sample notebook - [Process Tree
Visualization](https://github.com/microsoft/msticpy/blob/master/docs/notebooks/ProcessTree.ipynb)

### Event timeline

:py`msticpy.nbtools.timeline`{.interpreted-text role="mod"} - event
timeline visualization.

See `../visualization/EventTimeline`{.interpreted-text role="doc"}

Sample notebook - [Event Timeline
Visualization](https://github.com/microsoft/msticpy/blob/master/docs/notebooks/EventTimeline.ipynb)

Data sub-package - *data*
-------------------------

See `../DataAcquisition`{.interpreted-text role="doc"}

### QueryProvider

:py`msticpy.data.data_providers.QueryProvider`{.interpreted-text
role="mod"}

Extensible query library targeting Log Analytics or OData endpoints.
Built-in parameterized queries allow complex queries to be run from a
single function call. Add your own queries using a simple YAML schema.

See `../data_acquisition/DataProviders`{.interpreted-text role="doc"}

Sample notebook - [Data Queries
Notebook](https://github.com/microsoft/msticpy/blob/master/docs/notebooks/Data_Queries.ipynb)

------------------------------------------------------------------------

::: {.note}
::: {.title}
Note
:::

The following modules are currently part of the `nbtools` sub-package
but will be moved to the `data` package.
:::

### SecurityAlert and SecurityEvent

:py`msticpy.nbtools.security_alert.SecurityAlert`{.interpreted-text
role="class"}

:py`msticpy.nbtools.security_event.SecurityEvent`{.interpreted-text
role="class"}

Encapsulation classes for alerts and events. Each has a standard
\'entities\' property reflecting the entities found in the alert or
event. These can also be used as meta-parameters for many of the
queries. For example the query:
`qry.list_host_logons(query_times, alert)` will extract the value for
the `hostname` query parameter from the alert.

### Entities

:py`msticpy.nbtools.entity_schema`{.interpreted-text role="mod"}

Entity classes (e.g. Host, Account, IPAddress) used in Azure Security
Center and Azure Sentinel alerts and in many of the modules of
*msticpy*.

Each entity encapsulates one or more properties related to the entity.

------------------------------------------------------------------------

To-Do Items
-----------

-   Add additional notebooks to document use of the tools.
-   Expand list of supported TI provider classes.
-   Expand Azure data enrichment options.

Supported Platforms and Packages
--------------------------------

-   msticpy is OS-independent
-   Requires Python 3.6 or later
-   Requires the following python packages: pandas, bokeh, matplotlib,
    seaborn, setuptools, urllib3, ipywidgets, numpy, attrs, requests,
    networkx, ipython, scikit\_learn, typing
-   The following packages are recommended and needed for some specific
    functionality: Kqlmagic, maxminddb\_geolite2, folium, dnspython,
    ipwhois

See [requirements.txt](requirements.txt) for more details and version
requirements.

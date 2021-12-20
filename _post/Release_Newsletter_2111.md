# Apache Ignite 2.11.1: Emergency Log4j2 Update

The new [Apache Ignite](https://ignite.apache.org/) 2.11.1 is an emergency release that fixes 
[CVE-2021-44228](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-44228), 
[CVE-2021-45046](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-45046),
[CVE-2021-45105](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-45105) related to the ignite-log4j2 module usage.

## Apache Ignite with Log4j Vulnerability

All the following conditions must apply:

* the Apache Ignite version lower than 2.11.0 is used (2.11.1, 2.12 and upper already fixes these vulnerabilities);
* the `ignite-logj42` is used by Apache Ignite and located in the `libs` directory (by default it is located in the `libs/optional` 
directory, so these deployments are not affected);
* The Java version in use is older than the following versions: `8u191`, `11.0.1`. This is due to the fact that later versions 
set the JVM property `com.sun.jndi.ldap.object.trustURLCodebase` to `false` by default, which disables JNDI loading of classes 
from arbitrary URL code bases. 

NOTE: Relying only on a Java version as protection against these vulnerabilities is very risky and haven't been tested.

## Risk Mitigation Without Upgrading

Please, note that all of these cases are required a cluster downtime, and it is still preferable for the Apache Ignite to be upgraded.

### Method 1: Removing The Vulnerable Classes

When using an older Apache Ignite version, it is possible to remove the JndiLookup class from any Java applications by 
executing this command:

```shell
find $IGNITE_HOME/ -type f -name "*log4j-core-*.jar" -exec zip -q -d "{}" org/apache/logging/log4j/core/lookup/JndiLookup.class \;
```

This will recursively find all log4j-core JAR files, starting from the `IGNITE_HOME` directory, and remove the vulnerable 
JndiLookup class from them.

### Method 2: Disabling Message Lookups

This method can be used as an additional protection layer in case you suspect not all log4j dependencies have been 
properly updated. If using the Apache Ignite older version, it is recommended disabling message lookups globally by 
setting the environment variable `LOG4J_FORMAT_MSG_NO_LOOKUPS` to `true` or, alternatively, run the Apache Ignite with 
the `‐Dlog4j2.formatMsgNoLookups=true` command-line option.

### Method 3: Replace log4j2 Dependency Manually

It is still possible to replace manually the Log4j 2.x versions in the Apache Ignite binary distribution to the `2.17.0` Log4j version 
if you log configuration doesn't imply to use the RoutingAppender (in other case the RoutingAppender usage will generate garbage 
in your logs).

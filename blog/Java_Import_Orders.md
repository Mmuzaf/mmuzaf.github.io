## The Classes Import Order For Java Projects

This is a list of the most popular projects (most of them are [open-source and  written in Java](https://projects.apache.org/projects.html?language#Java)) for viewing what kind of classes import order they use according to an appropriate `checkstyle` configuration.

### IntelliJ IDEA Default (tested on 2022.2)

```
java.*
javax.*
static all other imports
all other imports
```

### Eclipse Default (tested on 2022‑12)

```
static all other imports
[blank line]
java.*
[blank line]
javax.*
[blank line]
org.*
[blank line]
com.*
[blank line]
all other imports
```

### Checkstyle Default (tested on 8.40)


```
com.*
java.*
javax.*
org.*
all other imports
static all other imports
```

### Spring Framework

https://github.com/spring-projects/spring-framework/wiki/IntelliJ-IDEA-Editor-Settings#imports


```
java.*
[blank line]
javax.*
[blank line]
all other imports
[blank line]
org.springframework.*
static all other imports
```

### Apache HBase

https://github.com/apache/hbase/blob/master/hbase-checkstyle/src/main/resources/hbase/checkstyle.xml#L74

```
static all other imports
[blank line]
all other imports
[blank line]
org.apache.hbase.thirdparty
[blank line]
org.apache.hadoop.hbase.shaded
```

### Apache Flink

https://flink.apache.org/contributing/code-style-and-quality-formatting.html#imports

```
org.apache.flink.*
[blank line]
org.apache.flink.shaded.*
[blank line]
all other imports
[blank line]
java.*
[blank line]
javax.*
[blank line]
scala*
[blank line]
static all other imports
```


### Apache Zookeeper

https://github.com/apache/zookeeper/blob/master/checkstyle-strict.xml#L88

```
static all other imports
[blank line]
all other imports
```

### Apache Kafka

https://issues.apache.org/jira/browse/KAFKA-10787

```
org.apache.kafka.*
[blank line]
com.*
net.*
org.*
[blank line]
java.*
javax.*
[blank line]
all other imports
[blank line]
static all other imports
```

### Apache Storm

https://github.com/apache/storm/blob/master/storm-checkstyle/src/main/resources/storm/storm_checkstyle.xml#L220

```
static all other imports
[blank line]
all other imports
```

### Apache Ignite

https://github.com/apache/ignite/blob/master/checkstyle/checkstyle.xml#L47

```
java.*
[blank line]
javax.*
[blank line]
all other imports
[blank line]
static all other imports
```

### Apache Tomcat

https://github.com/apache/tomcat/blob/main/res/checkstyle/checkstyle.xml#L71

```
java.*
[blank line]
javax.*
[blank line]
jakarta.*
[blank line]
org.hamcrest.*
[blank line]
org.junit.*
[blank line]
org.*
[blank line]
...
all other imports
[blank line]
static all other imports
```

### Apache Calcite

https://gist.github.com/gianm/27a4e3cad99d7b9b6513b6885d3cfcc9#file-calcite-intellij-style-xml-L16

```
org.apache.*
[blank line]
com.*
[blank line]
all other imports
[blank line]
java.*
[blank line]
static all other imports
```
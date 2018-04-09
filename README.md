# Pulsar Functions Java Starter

This repo houses some example [Pulsar Functions](http://pulsar.incubator.apache.org/docs/latest/functions/overview/) written in Java. These functions can all be run either in [local mode](http://pulsar.incubator.apache.org/docs/latest/functions/overview#local-run) or in [cluster mode](http://pulsar.incubator.apache.org/docs/latest/functions/overview#cluster-mode).

## Requirements

You'll need to have [Maven](https://maven.apache.org) installed.

## Setup

Pulsar Functions written in Java need to be packaged as a "fat" jar. To create a fat jar:

```bash
$ mvn package
```

To deploy and run the functions in this repo, you'll need to navigate to the [Pulsar repo](https://github.com/apache/incubator-pulsar) and use the `pulsar-admin` executable in the `bin` folder. We recommend setting the path to the jar file created in this repo as an environment variable:

```bash
$ export PULSAR_FUNCTIONS_JAR=`pwd`/target/pulsar-functions-0.1.0-SNAPSHOT-jar-with-dependencies.jar
$ cd /path/to/incubator-pulsar

# Example create command
$ bin/pulsar-admin functions create \
  --jar $PULSAR_FUNCTIONS_JAR \
  --className pulsarfunctions.starter.SimpleStringFunction \
  --inputs persistent://sample/standalone/ns1/in \
  --output persistent://sample/standalone/ns1/out \
  --fqfn sample/ns1/simple-string
```

## Included functions

The following example functions are included in this repo:

Function name | Description | API used
:-------------|:------------|:--------
[`SimpleStringFunction`](src/main/java/pulsarfunctions/starter/SimpleStringFunction.java) | Ignores the input to the function and simply outputs the string `"This is my function!"` every time a message arrives | [Java native API](http://pulsar.incubator.apache.org/docs/latest/functions/api#java-native)
[`ContextFunction`](src/main/java/pulsarfunctions/starter/ContextFunction.java) | Fetches a wide variety of information from the function's context (tenant, namespace, message ID, etc.) and logs that information | [Pulsar Functions SDK for Java](http://pulsar.incubator.apache.org/docs/latest/functions/api#java-sdk)
[`CustomSerDeFunction`](src/main/java/pulsarfunctions/starter/CustomSerDeFunction.java) | This function uses a custom SerDe class ([`TweetSerDe`](src/main/java/pulsarfunctions/starter/serde/TweetSerDe.java)) for serializing and deserializing [`Tweet`](src/main/java/pulsarfunctions/starter/serde/Tweet.java) objects | [Java native API](http://pulsar.incubator.apache.org/docs/latest/functions/api#java-native)
# TDP Tez Notes

The version 0.9.1-TDP-0.1.0-SNAPSHOT of Apache Hive is based on the `branch-0.9.1` tag of the Apache [repository](https://github.com/apache/tez/tree/branch-0.9.1).

## Jenkinfile

The file `./Jenkinsfile-sample` can be used in a Jenkins / Kubernetes environment to build and execute the unit tests of the Tez project. See []() for details on the environment.

## Making a release

```
mvn clean install -pl \!tez-ui -Phadoop28 -P\!hadoop27 -DskipTests
```

The command generates a `.tar.gz` file of the release at `./tez-dist/target/tez-0.9.1-TDP-0.1.0-SNAPSHOT.tar.gz`.

## Testing parameters

```
mvn test -pl \!tez-ui -Phadoop28 -P\!hadoop27 -DskipTests --fail-never
```

- -pl \!tez-ui: Disable `tez-ui` maven project
- -Phadoop28: Activates the necessary Maven profile for Hadoop > 2.8 (as mentionned in `tez/BUILDING.txt`)
- -P\!hadoop27: De-activates the Maven profile for Hadoop < 2.8
- --fail-never: Does not interrupt the tests if one module fails

## Test execution notes

See `./test_notes.txt`

## Build notes

Commit `1a88da0221fb822e319ed81c8410143217336e4d` ([TEZ-3929](https://issues.apache.org/jira/browse/TEZ-3929)) backported to avoid an issue with incompatible versions of jersey-client (see `hive/tdp/README.md` for details).

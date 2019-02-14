**This is a magical folder used by Maven.**

You must have Maven 3.3.0+ for this to have any effect.
It applies for every project in subtree rooted at folder where the `.mvn` folder is located.

It may contain these magic files:
* `maven.config`: command line options for (every) mvn command
* `jvm.config`: JVM options appended to `MAVEN_OPTS`, applies to every mvn command
* `extensions.xml`: extra maven extensions
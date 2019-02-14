- *versioner* is common pom parent for every other pom, it only defines flatten and other version-related stuff (not in this example)
- *bom* is import type pom which contains versions of all the internal libs and 3rd party dependencies so they don't have to be repeated all over the place
- *parent* pom from which other lib and app poms inherit, contains our custom build definition
- *libN* some internal libraries which have dependencies among themselves, they transitively see proper version from the bom

Parent has to depend on bom (to force build order) and import it in dependency management so other poms don't have to.

If I run `mvn clean install` with flatten plugin in version `1.1.0` I get
```
[ERROR] Failed to execute goal org.codehaus.mojo:flatten-maven-plugin:1.1.0:flatten (flatten-pom) on project parent: 1 problem was encountered while building the effective model for toy:parent:0
[ERROR] [FATAL] Non-readable POM .../repository/toy/bom/0/bom-0.pom: .../repository/toy/bom/0/bom-0.pom (No such file or directory) @ .../repository/toy/bom/0/bom-0.pom
```
which I interpret as that plugin not using jgitver during model read.

With flatten plugin in version `1.2.0-SNAPSHOT` (code from PR#86) the build runs fine.

As for why we use custom flatten, if the project is built without it depending only on jgitver all poms but the top level aggregator one are installed with version `0`.
See installed poms after removing `<skipPomUpdate>true</skipPomUpdate>` in config, disabling flatten plugin execution in `versioner` and running with `-Djgitver.flatten=true`.

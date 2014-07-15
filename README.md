## Gradle Release Plugin Fork - Gradle 2.0, Java 8, maven-publish support

This a fork of the townsfolk gradle release plugin -> [https://github.com/townsfolk/gradle-release](https://github.com/townsfolk/gradle-release).

If you are using the 'uploadArchives' method of publishing artifacts I would stick with the original plugin. 
I created this version specifically to add support for the newer 'maven-publish' method. 

Key changes:

* Made some changes to get the unit tests to pass with Java 8 and Gradle 2.0
* Added two new composite tasks to the ReleasePlugin that breaks the current 'release' task in two:
  * releasePrepare: Does everything up to and including 'preTagCommit'.
  * releasePerform: Does the remaining steps.


## Howto use

1. Checkout the project and change the group id in 'gradle.properties' to something useful.
2. Run 'gradle publishToMavenLocal' to get yourself a local copy of the plugin.
3. In the project for which you want to use the release plugin modify your build.gradle along the lines of
[https://github.com/jousby/gradle-release-example](https://github.com/jousby/gradle-release-example)
4. Then run the the two new release tasks in two subsequent gradle calls:
  * `gradle releasePrepare`
  * `gradle releasePerform`
  
## Why two steps?

Lets say your project version is 1.0-SNAPSHOT and you have tied 'publish' to 'createReleaseTag' as per the
old uploadArchives method. If you run the release plugin it will bump the version number to '1.0' and build
a 1.0 artifact however the 'publish' task seems to stick with the project.version that was in place when
gradle launches and the publish task ends up pushing a '1.0-SNAPSHOT' artifact to your remote repository.

The simple hack was to do something similar to the maven release plugin. Bump the version in one step. Then 
do a subsequent build and publish at the target release version in a follow up step. 

Happy to be pointed at solution that moves us back being able to do this one step but this was fairly 
painless.


# Project Release Guidelines

This guide documents the release process steps that are recommended to use when creating Spark Technology Center project
releases. Note that we are adopting the same release process as the one used by The Apache Software Foundation,
so this guide can also serve as a guide for releasing Apache projects.

The following items will be discussed in this document :

* [Prerequesites for Release Managers](#Prerequesites+for+Release+Managers)
* [Release Policies](#Release+Policies)
* [Release Automation Script](#Release+Automation+Script)
* [Preparing the release](#Preparing+the+release)
* [Publishing the  release](#Publishing+the+release)
* [The Release VOTE](#The+Release+VOTE)
* [Publishing and Announcing the new project release](#Publishing+and+Announcing+the+new+project+release)


## Prerequesites for Release Managers

### Creating GPG Signing key

Every release manager MUST create it's own Signing Key that is going to be used to signing all release artifacts.

Please follow [these steps](https://cwiki.apache.org/confluence/display/TUSCANYxDOCx2x/Create+Signing+Key) to create your signing key.

### Configuring maven credentials for staging repositories

Releases are published published into Maven repositories for public consumption. Release managers need to configure their local environment with proper credentials to enable the maven plugins to update/upload the release artifacts to these repositories.

Please ensure you have your ~/.m2/settings.xml properly configured as per [these instructions](http://maven.apache.org/developers/committer-settings.html).

**Note:** that Apache and Sonatype Maven Central repositories have different configuration URLs, so depending on where you are releasing you might need to configure different repository/credentials.


## Release Policies

Release managers should be very familiar with some of the following policies and guidelines before creating their first release:

* [Apache Release Distribution Policy](https://www.apache.org/dev/release-distribution.html)
* [Apache Incubator Release Management Guidelines] (http://incubator.apache.org/guides/releasemanagement.html)

## Release Automation Script

The steps below will assume the usage of a [release script](https://github.com/apache/incubator-systemml/blob/master/dev/release/release-build.sh) to automate repetitive release steps.

For the release script example above, you have the following options available :

```
usage: release-build.sh [--release-prepare | --release-publish | --release-snapshot]

DESCRIPTION

Use maven infrastructure to create a project release package and publish
to staging release location (https://dist.apache.org/repos/dist/dev/incubator/systemml/)
and maven staging release repository.

--release-prepare --releaseVersion="0.11.0-incubating" --developmentVersion="0.11.0-SNAPSHOT" [--releaseRc="rc1"] [--tag="v0.11.0-incubating"] [--gitCommitHash="a874b73"]
This form execute maven release:prepare and upload the release candidate distribution
to the staging release location.

--release-publish --gitCommitHash="a874b73"
Publish the maven artifacts of a release to the Apache staging maven repository.

--release-snapshot [--gitCommitHash="a874b73"]
Publish the maven snapshot artifacts to Apache snapshots maven repository

OPTIONS

--releaseVersion     - Release identifier used when publishing
--developmentVersion - Release identifier used for next development cyce
--releaseRc          - Release RC identifier used when publishing, default 'rc1'
--tag                - Release Tag identifier used when taging the release, default 'v$releaseVersion'
--gitCommitHash      - Release tag or commit to build from, default master HEAD
--dryRun             - Dry run only, mostly used for testing.

A GPG passphrase is expected as an environment variable

GPG_PASSPHRASE - Passphrase for GPG key used to sign release
```


## Preparing the release

The release-prepare option in the release automation script will execute the following tasks :

* Create a Release Tag for the release with proper release version
* Update the code in Master branch to the next development version

```
release-build.sh --release-prepare \
   --releaseVersion="0.12.0-incubating" \
   --developmentVersion="0.13.0-SNAPSHOT" \
   --releaseRc="rc1" \
   --tag="v0.12.0-incubating-rc1"
```

After this step is executed, please verify the following have been processed properly:

* The release tag has been created at : https://github.com/ORG/PROJECT/releases
* The pom, in the release tag nas been updated to the proper "release version"
* The pom, in the release tag nas been updated to the proper new "development version"

## Publishing the release

The release-publish option in the release automation script will execute the following tasks :

* Publish maven artifacts to the maven staging repository
* Publish release artifacts to the staging release location

```
release-build.sh --release-publish \
   --gitTag="v0.12.0-incubating-rc1"
```


After this step is executed, please verify the following have been processed properly:

* Maven artifacts are correctly available at the Maven Staging Repository
  ** For Maven Central, the staging repositories are located at [https://oss.sonatype.org/#stagingRepositories](https://oss.sonatype.org/#stagingRepositories)
  ** For Apache Projects, the staging repositories are located at [https://repository.apache.org/#stagingRepositories](https://repository.apache.org/#stagingRepositories)
* Release artifacts are correctly available at the release staging repository
 ** For Apache Projects, the release staging location is [https://dist.apache.org/repos/dist/dev/incubator/PROJECT/RELEASE-VERSION](https://dist.apache.org/repos/dist/dev/incubator/PROJECT/RELEASE-VERSION)

**Note:** For now, the maven staging repository needs to be manually closed by following the steps below :

* Login to [https://repository.apache.org/](https://repository.apache.org/)
* Select your Staging repository at [https://repository.apache.org/#stagingRepositories](https://repository.apache.org/#stagingRepositories)
* Select **Close** button at the top of the UI and provide a description for the repository (e.g. Project Foo RELEASE-VERSION RELEASE-CANDIDATE-NUMBER)


## The Release VOTE

Now that all the release artifacts are in-place, the Release Manager needs to submit an Approval Vote e-mail similar to the one below

```
TO: project-dev-list-email
SUBJECT: [VOTE] PROJECT RELEASE_VERSION (RELEASE-CANDIDATE-NUMBER)

Please vote on releasing the following candidate as PROJECT version RELEASE-VERSION!


The vote is open for at least 72 hours and passes if a majority of at least
3 +1 PMC votes are cast.

[ ] +1 Release this package as PROJECT RELEASE_VERSION
[ ] -1 Do not release this package because ...

To learn more about PROJECT, please see PROJECT-WEB-SITE

The tag to be voted on is TAG-NAME (GIT-HASH-FOR-THE-TAG)

https://github.com/ORGANIZATION/PROJECT/tree/GIT-HASH-FOR-THE-TAG

The release artifacts can be found at :

https://dist.apache.org/repos/dist/dev/incubator/PROJECT/RELEASE-VERSION-RCX/

The maven release artifacts, including signatures, digests, etc. can be
found at:

https://repository.apache.org/content/repositories/MAVEN-STAGING-REPOSITORY-ID/

```

For a concrete example, please see mailing-list archives for previous [voting threads](https://www.mail-archive.com/dev@systemml.incubator.apache.org/msg01097.html)

After the voting period ends and the vote passes, please submit a vote result e-mail with the vote tally.

```
TO: project-dev-list-email
SUBJECT: [RESULT][VOTE] PROJECT RELEASE_VERSION (RELEASE-CANDIDATE-NUMBER)

Please vote on releasing the following candidate as PROJECT version RELEASE-VERSION!


The vote is open for at least 72 hours and passes if a majority of at least
3 +1 PMC votes are cast.

[ ] +1 Release this package as PROJECT RELEASE_VERSION
[ ] -1 Do not release this package because ...

To learn more about PROJECT, please see PROJECT-WEB-SITE

The tag to be voted on is TAG-NAME (GIT-HASH-FOR-THE-TAG)

https://github.com/ORGANIZATION/PROJECT/tree/GIT-HASH-FOR-THE-TAG

The release artifacts can be found at :

https://dist.apache.org/repos/dist/dev/incubator/PROJECT/RELEASE-VERSION-RCX/

The maven release artifacts, including signatures, digests, etc. can be
found at:

https://repository.apache.org/content/repositories/MAVEN-STAGING-REPOSITORY-ID/

Vote has passed with N +1 binding votes from :

User 1
User 2
User 3

And N +1 (non-binding) votes from :

User 4
User 5

```

For a concrete example, please see mailing-list archives for previous [result voting threads](https://www.mail-archive.com/dev@systemml.incubator.apache.org/msg01119.html)

### The IPMC Release VOTE

For Apache Incubator projects, after a successful project vote, there is a need for another vote that needs to seek IPMC approval


```
TO: general@apache.org
SUBJECT: [VOTE] PROJECT RELEASE_VERSION (RELEASE-CANDIDATE-NUMBER)

Please vote on releasing the following candidate as PROJECT version RELEASE-VERSION!

The PPMC vote thread:
VOTE-LINK

And the result:
VOTE-RESULT-LINK

The vote is open for at least 72 hours and passes if a majority of at least
3 +1 PMC votes are cast.

[ ] +1 Release this package as PROJECT RELEASE_VERSION
[ ] -1 Do not release this package because ...

To learn more about PROJECT, please see PROJECT-WEB-SITE

The tag to be voted on is TAG-NAME (GIT-HASH-FOR-THE-TAG)

https://github.com/ORGANIZATION/PROJECT/tree/GIT-HASH-FOR-THE-TAG

The release artifacts can be found at :

https://dist.apache.org/repos/dist/dev/incubator/PROJECT/RELEASE-VERSION-RCX/

The maven release artifacts, including signatures, digests, etc. can be
found at:

https://repository.apache.org/content/repositories/MAVEN-STAGING-REPOSITORY-ID/

```


For a concrete example, please see mailing-list archives for previous [vote threads](https://www.mail-archive.com/general@incubator.apache.org/msg57019.html)

After the voting period ends and the vote passes, please submit a vote result e-mail with the vote tally.

For a concrete example, please see mailing-list archives for previous [vote threads](https://www.mail-archive.com/general@incubator.apache.org/msg57295.html)

## Publishing and Announcing the new project release

```
COMMING SOON
```


# Project open sourcing guidelines

This guide describe some best practices to be considered before making an approved open source project public.

* [Getting started](#getting-started)
* [Project License](#project-license)
  * [Applying a License](#applying-a-license)
  * [Validating license](#validating-license)
    + [Apache RAT](#apache-rat)
    + [Scala projects using Scalastyle checks](#scala-projects-using-scalastyle-checks)
  * [3rd Party Dependencies and License compatibility](#3rd-party-dependencies-and-license-compatibility)
* [Accepting third party contributions](#accepting-third-party-contributions)
  * [Test coverage and Continuous Integration](#test-coverage-and-continuous-integration)
* [Final Checklist](#final-checklist)

# Getting started

Multiple times when we are working on a project that aims to be open sourced
there is a need for external collaboration, in this case, GitHub private repositories
are very helpful as they give you the flexibility and privacy to perform the collaboration,
review, etc with folks outside of the organization. Note that private repositories are
only available for paid users, so you will have to create an organization or use your own
account and enable payment via settings. More details about this can be found at the
[introducing unlimited private repositories blog](https://github.com/blog/2164-introducing-unlimited-private-repositories) or on the [github pricing page](https://github.com/pricing)

# Project License

What people will be able to do with your software is governed by how your project is Licensed. When
we call a project **open source**, most of the times we are implying that the project license comply
with the [open source definition](https://opensource.org/osd) -  in brief, they allow software to be freely used, 
modified, and shared. The [Open Source Initiative - OSI] (https://opensource.org/licenses) has a list
of [approved open source licenses](https://opensource.org/licenses/category

In general, licenses will be divided in three main categories :

* permissive: The permissive licenses, as the name describe, are very permissive permitting that the software
even become proprietary (e.g. Public Domain, MIT, BSD-new, Apache License 2.0).

* strongly-protective or strong-copyleft: These licenses will not permit the software to become proprietary
(e.g. GPL, GPL2, GPL3, AGPL3)

* weakly-protective or weak-copyleft: These licenses are a compromise between the two categories described
above, restricting the software to become proprietary but enabling it to be part of a larger proprietary
work.

As I have been working at The Apache Software Foundation for over 10 years, I am going to focus on the
[Apache License 2.0](http://www.apache.org/licenses/) and how to apply it to your project. Note that,
although I am focusing on a specific license, most of the steps would apply to other licenses with
minimum modification.

## Applying a License

Let's take the Apache License 2.0 as an example, and examine the steps necessary to apply it to a given
project.

*Adding the license file: consist in creating a LICENSE file (or LICENSE.md) and copying the license
contents to the newly created file. Note that Github also provide some tools to apply a license to
your project that is described in the [Adding a license to a repository](https://help.github.com/articles/adding-a-license-to-a-repository/) article.

*Adding license headers: consist in applying the license recommended boilerplate notice in all files.

```bash
Copyright [yyyy] [name of copyright owner]

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```

## Validating license

There are automated tools that help validating that all artifacts in a project is properly complying with license
requirements. In the case of Apache License 2.0, these tools will validate that the files properly have the Apache
License 2.0 header.


### Apache RAT

For command-line validation and/or projects that have a maven build, [Apache Rat](https://creadur.apache.org/rat/)
might be a viable solution:
 
* Using command line


```bash
 cd opensource/project
 java -jar ~/opt-common/apache-rat-0.12.jar .
```


* Maven

```xml
<profile>
    <id>rat</id>
    <build>
        <defaultGoal>clean org.apache.rat:apache-rat-plugin:check</defaultGoal>
        <plugins>
            <plugin>
                <groupId>org.apache.rat</groupId>
                <artifactId>apache-rat-plugin</artifactId>
                <version>0.11</version>
                <executions>
                    <execution>
                        <phase>package</phase>
                        <goals>
                            <goal>check</goal>
                        </goals>
                    </execution>
                </executions>
                <configuration>
                    <excludes>
                        <exclude>.gitignore</exclude>
                        <exclude>.repository/</exclude>
                        <exclude>.idea/</exclude>
                        <exclude>.git</exclude>
                        <exclude>.settings</exclude>
                        <exclude>.classpath</exclude>
                        <exclude>.project</exclude>
                        <exclude>docs</exclude>
                        <exclude>**/docs/**</exclude>
                        <exclude>**/*.csv</exclude>
                        <exclude>**/*.ijv</exclude>
                        <exclude>**/*.json</exclude>
                        <exclude>**/*.mtx</exclude>
                        <exclude>**/*.mtd</exclude>
                        <exclude>**/part-*</exclude>
                        <exclude>**/*.keep</exclude>
                        <exclude>**/target/**</exclude>
                        <exclude>**/README.md</exclude>
                        <exclude>**/*.svg</exclude>
                        <!-- Jupyter Notebooks -->
                        <exclude>**/*.ipynb</exclude>
                        <!-- Generated antlr files -->
                        <exclude>src/main/java/*.tokens</exclude>
                        <!-- Generated python files -->
                        <exclude>src/main/python/systemml.egg-info/**</exclude>
                        <!-- Sphinx reStructuredText files -->
                        <exclude>src/main/pythondoc/*.rst</exclude>
                        <!-- Compiled ptx file from nvcc -->
                        <exclude>src/main/cpp/kernels/SystemML.ptx</exclude>
                        <!-- Proto files -->
                        <exclude>src/main/proto/caffe/caffe.proto</exclude>
                        <exclude>scripts/nn/examples/caffe2dml/models/**/*</exclude>
                        <!-- Test Validation files -->
                        <exclude>src/test/scripts/functions/jmlc/**/*.impute</exclude>
                        <exclude>src/test/scripts/functions/jmlc/**/*.map</exclude>
                        <exclude>src/test/scripts/functions/jmlc/**/*.mode</exclude>
                        <exclude>src/test/scripts/functions/jmlc/**/*.ndistinct</exclude>
                        <exclude>src/test/scripts/functions/jmlc/**/*.node</exclude>
                        <exclude>src/test/scripts/functions/jmlc/tfmtd_example/Bin/saleprice.bin</exclude>
                        <exclude>src/test/scripts/functions/jmlc/tfmtd_example/Bin/sqft.bin</exclude>
                        <exclude>src/test/scripts/functions/jmlc/tfmtd_example/column.names</exclude>
                        <exclude>src/test/scripts/functions/jmlc/tfmtd_example/dummycoded.column.names</exclude>
                        <exclude>src/test/scripts/functions/jmlc/tfmtd_example2/column.names</exclude>
                        <exclude>src/test/scripts/functions/jmlc/tfmtd_frame_example/tfmtd_frame</exclude>
                        <!-- Perftest requirement file -->
                        <exclude>scripts/perftest/python/requirements.txt</exclude>
                    </excludes>
                </configuration>
            </plugin>
        </plugins>
    </build>
</profile>

```


* Generated report

```bash
*****************************************************
Summary
-------
Generated at: 2018-01-21T21:04:37-08:00

Notes: 2
Binaries: 0
Archives: 0
Standards: 87

Apache Licensed: 78
Generated Documents: 0

JavaDocs are generated, thus a license header is optional.
Generated files do not require license headers.

9 Unknown Licenses

*****************************************************

Files with unapproved licenses:

  ./.gitattributes
  ./.gitignore
  ./README.md
  ./release-notes.md
  ./build/checkstyle/code_checks.xml
  ./build/findbugs/findbugs-exclude.xml
  ./conf/core-site.xml.template
  ./src/main/resources/stocator.properties
  ./src/test/resources/core-site.xml.template

*****************************************************
```

### Scala projects using Scalastyle checks

For Scala projects, Scalastyle provides an easy way to validate files have proper license headers by incorporating the
check below into the "scalastyle-config.xml"

```xml
 <check level="error" class="org.scalastyle.file.HeaderMatchesChecker" enabled="true">
  <parameters>
   <parameter name="header"><![CDATA[/*
 * Copyright [yyyy] [name of copyright owner]
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *     http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 *
 */]]></parameter>
  </parameters>
 </check>
```

## 3rd Party Dependencies and License compatibility

When we talk about license compatibility, we are describing the ability to combine multiple components
that are using different open source licenses in order to produce a large work.

The diagram below will help you identify what dependencies are fit to be included in your project based
on the license you have chosen to your project

![License Compatibility](https://www.dwheeler.com/essays/floss-license-slide-image.png)
Image by David A. Wheeler - https://www.dwheeler.com/essays/floss-license-slide.html


Based on the license you have chosen for your project, it's imperative that you validate all its dependencies
for license compatibility periodically and particularly before releases.  

# Accepting third party contributions

When a project accepts 3rd party contributions, there is a need to ensure that the contributions are valid and that 
the project will have the ability to package and redistribute those according to the project license. To accomplish this,
different projects have utilized different strategies, while Apache based projects requires a signed 
[CCLA](https://www.apache.org/licenses/cla-corporate.pdf) and/or 
[ICLA](https://www.apache.org/licenses/icla.pdf) for large contributions, other projects have adopted a simpler 
approach whic is to use a [Developer Certificate of Origin - DCO](https://developercertificate.org/) for each contribution.


For the purpose of making the project publicly available at GitHub, the DCO should suffice, and we can easily incorporate
the DCO text in each pull request by creating a pull request template containing the DCO text. To accomplish this, simply
create a **.github/PULL_REQUEST_TEMPLATE** file in your project with the DCO text contents below.


```text
 
Developer's Certificate of Origin 1.1

       By making a contribution to this project, I certify that:

       (a) The contribution was created in whole or in part by me and I
           have the right to submit it under the Apache License 2.0; or

       (b) The contribution is based upon previous work that, to the best
           of my knowledge, is covered under an appropriate open source
           license and I have the right under that license to submit that
           work with modifications, whether created in whole or in part
           by me, under the same open source license (unless I am
           permitted to submit under a different license), as indicated
           in the file; or

       (c) The contribution was provided directly to me by some other
           person who certified (a), (b) or (c) and I have not modified
           it.

       (d) I understand and agree that this project and the contribution
           are public and that a record of the contribution (including all
           personal information I submit with it, including my sign-off) is
           maintained indefinitely and may be redistributed consistent with
           this project or the open source license(s) involved.
```

#Documentation

We use the **[community over code](https://blogs.apache.org/foundation/entry/asf_15_community_over_code)** 
philosophy at Apache, and an easy way to start building communities around your project is to enable them to
self-promote themselves by providing a easy to read and organized documentation.

In small projects, that the only presence is its github repository, documentation can be made available via
[github pages](https://pages.github.com/) using simple to use jekyll templates and markdown markups.

For python based projects, there are other tools available like [Read the Docs](https://readthedocs.org/) 

As for suggested topics, I would recommend :

* Detailed build/install instructions available on README.md
* Getting started scenarios available in project documentation (github pages)
* Other advanced documentation, such as external APIs, algorithms exposed, etc


## Test coverage and Continuous Integration

Most open source projects today follows an agile methodology and rely on continuous integration to maintain its
code quality. 

Tools such as [Travis.CI](https://about.travis-ci.com/) enables validating each contribution before
it reaches your master repository by running integration test suits in a matrix of supported platforms and/or
language versions.

Note that, although the main purpose of the test suits are to maintain the project code quality and avoid regressions,
it also have a second purpose which is to provide detailed usage of your project code (e.g. apis, plugability, etc)

# Final Checklist

Before flipping the switch and make your project open source, check the following :

* LICENSE file is available in the root of the repository
* All required files have License headers as appropriate based on chosen license requirements
* All 3rd party dependencies are compatible with chosen license
* Pull request template is configured with Developer Certificate of Origin
* README.md file is available and contains detailed build/install instructions  
* Minimum documentation is available 

#DISCLAIMER

I am not a lawyer and the information provide here is merely information and does not constitute leval advice.


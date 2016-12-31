# Sandscape User Documentation

### About Sandscape

Sandscape is a set of [Jenkins Script Console][jenkins-sc] scripts which are
designed to run and configure a Jenkins instance.  It can run from [Jenkins init
hooks][jenkins-hook] (which runs every time Jenkins is restarted) or by calling
it via the Jenkins Script Console interactively.

Some advantages:

* Configuration is applied to the Jenkins runtime without requiring a restart.
* It has a pluggable architecture which allows Sandscape plugins to be
  dynamically loaded.  Sandscape plugins are the bits which actually configure
  Jenkins.

If you've not interacted with the Jenkins Script Console before, then I
encourage you to watch [this video][youtube-sc] presented and recorded by the
original author of Sandscape.

### Warnings and Recommendations

##### Warnings

It is _very_ *important* to understand all of the following points because
Sandscape operates within the Jenkins Script Console.  The Jenkins Script
Console:

* Is a web-based shell into the Jenkins runtime.  It is a Groovy Shell.  Groovy
  is a very powerful language which offers the ability to do practically
  anything Java can do including create sub-processes and execute arbitrary
  commands on the Jenkins master and slaves.  It can even read files in which
  the Jenkins master has access to on the host (like `/etc/passwd`).
* It offers no administrative controls stop a script once it is able to
  execute within the Script Console.
* Can configure any Jenkins setting and can do more than any human administrator
  could via the Jenkins Web UI.  It can disable security, reconfigure security,
  even open a backdoor on the host operating system completely outside of
  Jenkins.
* Is so powerful because it was originally intended as a debugging interface for
  Jenkins developers.

Do not underestimate the capabilities of the Jenkins Script Console.  Remember,
Sandscape runs within the Jenkins Script Console.

Sandscape plugins make use of classes from Jenkins plugins to configure Jenkins.
Becuase of this, minor updates to Jenkins plugins can easily break Sandscape
plugins.

##### Recommendations

Recommendations for Jenkins:

Due to the tight integration Sandscape plugins require with Jenkins plugins, it
is recommended to adopt a strict process of Jenkins plugin versioning and to
disable the [Jenkins Update Site][sc-disable-jus].  The Sandscape project offers
a template which can be used for this to [bootstrap Jenkins and exact versions
of plugins][ss-jenkins-bootstrap].  The Jenkins bootstrapper also supports
generating system packages for the Jenkins master.

Recommendations for Sandscape:

* It is _highly recommended_ that before you configure sandscape on your Jenkins
  instance, you review all of the source code.
* Before using any Sandscape plugin, you should completely review and understand
  its source code.  Sandscape plugins execute in the Jenkins Script Console as
  well so the same warnings above apply to them.
* Mirror the Sandscape source code into a place you fully control.  This way
  updates to Sandscape will not go unreviewed.
* Mirror any Sandscape plugin you plan to use into a place you fully control.
  This is so updates to the plugin sources don't affect your configuration and
  don't go unreviewed.

Recommendations for Sandscape plugins:

# Getting started

To set up Sandscape, clone [the Sandscape init repository][sc] and execute:

    git clone https://github.com/sandscape/sandscape.git
    cd sandscape/
    export JENKINS_CREDENTIALS="someuser:somepassword"
    export JENKINS_WEB="https://jenkins.example.com/"
    ./setup_sandscape.sh

`JENKINS_CREDENTIALS` must contain the credentials of a Jenkins administrator
and `JENKINS_WEB` is the location to an existing Jenkins instance.

Sandscape will not make changes to your Jenkins installation without a
`config.json` file, which dictates the configuration of a Jenkins instance.


[![Creative Commons License][cc-img]][cc-by-sa] This documentation is licensed
under a [Creative Commons Attribution-ShareAlike 4.0 International
License][cc-by-sa].  See [LICENSE](LICENSE) for details.

[cc-by-sa]: http://creativecommons.org/licenses/by-sa/4.0/
[cc-img]: https://i.creativecommons.org/l/by-sa/4.0/80x15.png
[jenkins-hook]: https://wiki.jenkins-ci.org/display/JENKINS/Groovy+Hook+Script
[jenkins-sc]: https://wiki.jenkins-ci.org/display/JENKINS/Jenkins+Script+Console
[sc-disable-jus]: https://github.com/samrocketman/jenkins-script-console-scripts/blob/master/disable-all-update-sites.groovy
[sc]: https://github.com/sandscape/sandscape
[ss]: https://github.com/sandscape/sandscape
[youtube-sc]: https://www.youtube.com/watch?v=T1x2kCGRY1w

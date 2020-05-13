Getting Started With Jenkinsfile
===================================

Not explaining Jenkinsfile. There are plenty of good examples for it over the internet. This is just something I did when I was bored.

## Some links for more in depth learning
* [Learn Jenkinsfile](https://www.jenkins.io/doc/book/pipeline/jenkinsfile/) A website for learning about Jenkinsfile.
* [Openshift with Jenkinsfile](https://www.openshift.com/blog/deploying-jenkins-on-openshift-part-1) An Openshift blog about how to work on Jenkins with Openshift.


### Setting up
--------------
This is done on a CRC deployment of Openshift 4.4 on Fedora 30.


Things you'll need
--------------------
Openshift Cluster

For installing CRC Openshift 4 on your laptop, follow the following:

* [Setup CRC on your laptop](https://developers.redhat.com/blog/2019/09/05/red-hat-openshift-4-on-your-laptop-introducing-red-hat-codeready-containers/) A CRC blog from Red Hat.

Things you should have after following the steps.
--------------------
1. Access to a couple of projects


2. A working Jenkins server

Releasing
------------
Finished with your project?

- Create a feature branch as normal.
- Update the version history in the README.md file
- Update this to develop as normal.

		git checkout master
		git merge --no-ff develop
		git push origin master
		git tag v1.0.0
		git push origin v1.0.0

Replace 1.0.0 in the snippet here with your appropriate versions. Now you have a tag saved.

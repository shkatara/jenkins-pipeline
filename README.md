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
1. To get it working, you will need to setup your environment.

Once you have the required Openshift cluster setup by CRC or some other ways, create the project and deploy Jenkins-ephemeral from the template.

> shub ocp4.2 ~/sample-pipeline % oc new-project jenkins-ci-cd  \
> | shub ocp4.2 ~/sample-pipeline % oc new-app --name jenkins jenkins-ephemeral

Having the Jenkins server running, you need to create the project which Jenkins will ues to deploy applications on. Usually this could be the same project where Jenkins is running, but it is a common approach to have one Jenkins server running and sharing it between multiple projects, such as dev projects.

shub ocp4.2 ~/sample-pipeline % oc new-project jenkins-deploy

Because Jenkins is running in a different project and it will build and deploy applications in a different project, you will need to give at least edit access to the jenkins serviceaccount to your new jenkins-deploy project. 

shub ocp4.2 ~/sample-pipeline % oc policy add-role-to-user edit system:serviceaccount:jenkins-ci-cd:jenkins 

2. In the jenkins-ci-cd project, there is a route created for the Jenkins server, use that to open Jenkins. As this is running on Openshift, you can use the same credentials to login as your own openshift cluster, thanks to the Openshift Login Plugin. 

Login using your credentials and go to Manage Jenkins ( on the left side ) ---> Configure System.

Find the "OpenShift Jenkins Sync" project and just add the project you made in the "Namespace" field. Make sure you do not remove the existing project name. Use a whitespace as a delimiter. Do not change anything else unless you know what you are doing. Hit save. 

3. Now you will need to start a new build using the Jenkinsfile given in the git repository.

shub ocp4.2 ~/sample-pipeline % oc project jenkins-deploy
shub ocp4.2 ~/sample-pipeline % oc new-app --name from-jenkinsfile --code https://github.com/shkatara/jenkins-pipeline --context-dir shubham-pipeline

In some time, you should see a build starting in Openshift / Jenkins. If not, manually start it, 

shub ocp4.2 ~/sample-pipeline % oc start-build from-jenkinsfile

4. Look at the build, it will start the pipeline and the pipeline will then start the building and prompt for the inputs for creating the route for the built application. Approve it and your application should be ready in the jenkins-deploy project. 

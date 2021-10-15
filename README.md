# Deploy EAP app to OpenShift with external db (MySQL)

This repository contains the deployment for the EAP application deployed to OpenShift with external database (MySQL).

The EAP application code is maintained in https://github.com/deewhyweb/eap-quickstarts.git in the kitchensink folder.

The OpenShift objects in this repository will create two build configs and image streams.

1. Build the artifact image
2. Build the EAP application image

The first step is to build the artifact image, this is where the maven build of the EAP application is performed.

The second step is to build the EAP application image, this is where the EAP runtime image is created including the artifcact built in the previous step.
## pre-requisites

* OpenShift cluster with JBoss EAP operator installed
* Openshift command line tool installed and logged into cluster.

## Steps to deploy EAP app to OpenShift

Create the OpenShift project

`oc new-project eap-webinar`

Deploy mysql

`oc new-app  -e MYSQL_DATABASE=books -e MYSQL_PASSWORD=demo -e MYSQL_USER=eap mysql-persistent `

Create a pull secret with your registry.redhat.io credentials

`oc create secret docker-registry my-pull-secret --docker-server=registry.redhat.io --docker-username=$USERNAME --docker-password=$PASSWORD --docker-email=$EMAIL`


Install the helm cli following these instructions https://helm.sh/docs/intro/install/


Install the helm chart

`helm install kitchensink -f install-helm.yml http://github.com/openshift-helm-charts/charts/releases/download/redhat-eap74-1.1.0/redhat-eap74-1.1.0.tgz`


Wait for the two builds to complete, eap_app_build_artifacts and eap_app.  This will take a few minutes.

Create the config map with mysql environment variables;

`oc create -f eap-cm.yml`

Deploy the instance of the eap application using the operator

`oc create -f eap-deploy.yml`

Navigate to the route to test out the application.





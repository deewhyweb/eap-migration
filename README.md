# eap-migration

## pre-reqs

OpenShift cluster with JBoss EAP operator installed

## Steps

`oc new-project eap-webinar`

Deploy mysql

`oc new-app  -e MYSQL_DATABASE=books -e MYSQL_PASSWORD=demo -e MYSQL_USER=eap mysql-persistent `

Log in to the Red Hat Container Registry using your Customer Portal credentials to import the JBoss EAP imagestreams and templates. For more information, see Red Hat Container Registry Authentication. https://access.redhat.com/RegistryAuthentication

`oc create -f xxxxxx-secret.yaml`

Import required Images

```
oc replace --force -f \
https://raw.githubusercontent.com/jboss-container-images/jboss-eap-openshift-templates/eap74/eap74-openjdk11-image-stream.json
```

Create the artifact image stream

`oc create -f artifact-is.yml`

Create the eap application image stream

`oc create -f eap-is.yml`

Create the eap application build config

`oc create -f eap-bc.yml`

Create the artifact build config

`oc create -f artifact-bc.yml`

Wait for the two builds to complete, eap_app_build_artifacts and eap_app.  This will take a few minutes.

Create the config map with mysql environment variables;

`oc create -f eap-cm.yml`

Deploy the instance of the eap application using the operator

`oc create -f eap-deploy.yml`

Navigate to the route to test out the application.
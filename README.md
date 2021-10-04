# eap-migration

`oc new-project eap-webinar`

Log in to the Red Hat Container Registry using your Customer Portal credentials to import the JBoss EAP imagestreams and templates. For more information, see Red Hat Container Registry Authentication. https://access.redhat.com/RegistryAuthentication

`oc create -f 6340056_pheap-secret.yaml`

Import required Imanges

```
oc replace --force -f \
https://raw.githubusercontent.com/jboss-container-images/jboss-eap-openshift-templates/eap74/eap74-openjdk11-image-stream.json
```

`oc create -f artifact-is.yml`

`oc create -f artifact-bc.yml`


`oc create -f eap-is.yml`

`oc create -f eap-bc.yml`



`oc start-build eap-app-build-artifacts --follow`

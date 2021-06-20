Steps to run this Demo Code on Openshift.com
============================================

* Create free Trial Account at https://www.openshift.com

* Install OpenShift Client Tools (for Windows see: https://blog.openshift.com/installing-oc-tools-windows/)

* Open a command window and login to Openshift using your new account:
```bash
oc login https://api.starter-us-west-2.openshift.com 
```

* Create a new project:
```bash
oc new-project demojee
```

* Create an PostgreSQL Database:
```bash
oc new-app postgresql -e POSTGRESQL_USER=demo -e POSTGRESQL_PASSWORD=developer -e POSTGRESQL_DATABASE=demojee 
```

* Create JBoss/Wildfly Applicationserver instance including the Demo Application from this Git-Repo:
```bash
oc new-app wildfly:latest~https://github.com/FrankBergerITServices/openshift-jee-sample.git -e POSTGRESQL_USER=demo -e POSTGRESQL_PASSWORD=developer -e POSTGRESQL_DATABASE=demojee --name='wildflydemo'
```

* If you have not Wildfly-Image on your Openshift-Cluster I may have to create one first:
```bash
oc import-image wildfly:21.0 --confirm --from=quay.io/wildfly/wildfly-centos7:21.0 --insecure -n openshift
```

* To make our Demo Application accessible on the Internet we need to add a Route:
```bash
oc expose service wildflydemo
```

* You should now be able to access the URL of the created Route and you should reach our Demo Application if you add the Context "/demo/" to the URL

* To clean up after your tests, delete the project:
```bash
oc delete project demojee
```


Alternative (faster) Way using a Template
=========================================

* Create a new project:
```bash
oc new-project demojee
```

* Directly create the app by referencing the template:
```bash
oc new-app -f https://raw.githubusercontent.com/FrankBergerITServices/openshift-jee-sample/master/demojee.yaml
```

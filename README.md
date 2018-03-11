# microservice-builder

# Pre requisite
- IBM Cloud Privte Cloud Foundry environment
- IBM Bluemix CLI
- IBM Dev plugin

# create a microservice with BX CLI
```
bx dev create
```
Follow the command in console
```
Log in to Bluemix using the bx login command to synchronize your projects with the Bluemix dashboard, and to enable the use of Bluemix services in your project.
? Do you wish to continue without logging in? [y/n]> y
? Select a pattern:
1. Web App
2. Mobile App
3. Backend for Frontend
4. Microservice
Enter a number> 4

? Select a starter:
1. Basic
Enter a number> 1

? Select a language:
1. Java
2. Node
3.
4.
Enter a number> 1

? Enter a name for your project> javaservice
```

# Build the projects
```
cd javaservice
mvn install
bx dev run
```

# access the application
using a browser access
```
[local app url](http://localhost:9080)
```
## check application health status
```
[check app health](http://localhost:9080/javaservice/health)
```
## access the default API of the microservice
```
[RES API](http://localhost:9080/javaservice/v1/example)
```

## login to ICP private docker registery
```
docker login prd.icp:8500
```

## list images
```
docker images
```

## tag image
```
docker tag javaservice prd.icp:8500/dev/javaservice:v1.0
```

## push the image to icp
```
docker push prd.icp:8500/dev/javaservice:v1.0
```

# Deploy the image
from the menu Workloads/Deployments enter the following information in the 'Create Deployment' dialog.

General Tab

name | value
-----| -----
Name | js-javaservice

Container settings Tab

name | value
-----| -----
Name | js-javaservice
Image | prd.icp:8500/dev/javaservice:v1.0
Container port | 9080

# Expose the deployment with Service

General Tab

name | value
-----| -----
Name | js-javaservice-service
Type | NodePort

Ports Tab

name | value
-----| -----
TCP | http, 5000, 9080

Selectors Tab

name | value
-----| -----
app | js-javaservice

# Edit the source and rebuild, tag, and Push
You can edit the source code
- API Endpoint
- the health endpoint

## edit API Engpoint
edit the source **Example.java** located at folder /javaservice/src/main/java/application/rest/v1/

## edit HealthEndpoint
edit the source **HealthEndpoint.java** located at folder /javaservice/src/main/java/application/

## Build, Test, Push
To rebuild, and dockerize the image, run, once you are happy with the result locally you can push the image to ICP private repo.

```
mvn install
bx dev run
```
# Push the application to ICP CF

## set API endpoint
```
cf api https://api.mgmt.cf.sgcc.demo.lan --skip-ssl-validation
```
## login to ICP CF
```
cf login -u <user-id> -p  <user-password>
```

## Push the application
```
cf push javaservice -p target/javaservice-1.0-SNAPSHOT.zip -b https://github.com/cloudfoundry/java-buildpack.git
```

## list application running in CF
```
cf apps
```

With above you will see the list, find the application url and test it.
```
curl -i -H "Accept: application/json" -H "Content-Type: application/json" -X GET http://spring-boot-cli-application.apps.cf.sgcc.demo.lan/
```

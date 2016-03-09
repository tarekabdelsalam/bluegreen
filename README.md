# bluegreen
Simple demo code for Blue Green Deployments for Red Hat OpenShift 3.x.

AB Deployment Demo:

1. Create a new project for demo:

oadm new-project demo --display-name='Demo' --description='This is 

a demo project' --node-selector='region=primary' --admin=andrew

2. Switch to Andrew user:

su – Andrew

3. Login as Andrew:

oc login -u andrew --insecure-skip-tls-verify --

server=https://master.example.com:8443

4. Switch to the demo project:

oc project demo

5. Switch to Console and create a new app:

Paste https://github.com/tarekabdelsalam/ab-deploy.git into the new app

Appname: app-a

Route: empty

Label: abgroupmember=true

6. Get the dc:

oc get dc 

oc edit dc/app-a

copy abgroupmember: "true" to selector

oc expose dc/app-a --name=ab-service --

selector=abgroupmember=true --generator=service/v1

oc expose  service ab-service --name=ab-route --

hostname=abdeploy.cloudapps.example.com

oc scale dc/app-a --replicas=4

for i in  {1..10}; do curl abdeploy.cloudapps.example.com; echo " "; 

done

Create a new app ‘app-b’ with abgroupmember=true

Run 

for i in  {1..10}; do curl abdeploy.cloudapps.example.com; echo " "; 

done

do

oc scale dc/app-a --replicas=2

oc scale dc/app-b --replicas=2

run again:

oc scale dc/app-a --replicas=0

oc scale dc/app-b --replicas=4

run again:

for i in  {1..10}; do curl abdeploy.cloudapps.example.com; echo " "; 

done

# Swagger UI for OpenRiskNet VRE

This project is based on https://github.com/sabre1041/openshift-api-swagger

It provides the Swagger UI application that lets you navigate the REST API of applications.

The default is the OpenShift API of the site, though you should be able to look at other Swagger
endpoints.

To log in to the master API you need an authentication token. Do this either by logging in to the
OpenShift CLI and then executing `oc whoami -t` or by logging in to the OpenShift console, chosing
the `Ccopy Login Command` option from the user menu in the top right corner and copying out the
token from that login command.

The default endpoint for the application (e.g. for the OpenRiskNet reference site) is
http://swagger-ui.prod.openrisknet.org/

The location of the Swagger endpoint (e.g. for the OpenRiskNet reference site) is
https://prod.openrisknet.org/swagger.json

## Customisation

To customise this for your own VRE:

1. fork or clone this repo (or the orginal sabre1041/openshift-api-swagger repo).
2. edit `openshift-api-swagger-template.yml` and modify the location of the github repo to your fork or clone and (optionally) change the route name to something more reasonable (e.g. swagger-ui.your.site.org).
3. edit `swagger-ui-standalone-preset.js` and modify the default location of the swagger endpoint (hint: search for the `placeholder` property in that file), and optionally change the key from `placeholder` to `value` as the placeholder may not get used as the default.
4. push your changes to GitHub
5. run `oc process -f openshift-api-swagger-template.yml | oc create -f-` (NOTE: the instructions in the README.md document from the sabre1041/openshift-api-swagger repo are incorrect in this respect).
6. ssh to the master node and edit `/etc/origin/master-config.yaml`. Add your route name to the list of CORS origins in the corsAllowedOrigins section (you will add a line like this `- (?i)//swagger-ui\.prod\.openrisknet\.org(:|\z)`). Restart the origin-master-api service: `systemctl restart origin-master-api.service` 
7. after deployment is complete point your browser to http://swagger-ui.prod.openrisknet.org (or whatever is appropriate for your site)
8. once you are happy with things edit the route definition and change the value of the `kubernetes.io/tls-acme` anotation to `true` to enable trusted certificates
  




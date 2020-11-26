## OpenMRS Kubernetes Chart

This is a helm  chart for kubernetes that deploys https://hub.docker.com/r/openmrs/openmrs-reference-application-distro to kubernetes it should work with https://hub.docker.com/r/openmrs/openmrs-distro-platform or any distro generated via OpenmrsSDK it bundles mysql and is meant to be a turnkey deployment. The chart uses https://github.com/bitnami/charts/tree/master/bitnami/mysql/#installing-the-chart as a subchart so you can check it out in the link above.
### Minimum requirements to run

Create a values file
```yaml
env:
  dbAutoUpdate: "true"
  dbCreateTables: "true"
  dbDatabase: openmrs
  dbHost: '' #Ignored for now since we are using mysql subchart to be used when using external db
  dbPassword: Admin123
  dbUsername: openmrs
  moduleWebAdmin: "true"
ingress:
  enabled: true
  annotations:
    kubernetes.io/ingress.class: nginx
    kubernetes.io/tls-acme: "true"
    nginx.ingress.kubernetes.io/force-ssl-redirect: "true"
    cert-manager.io/cluster-issuer: "letsencrypt-prod"
    nginx.ingress.kubernetes.io/app-root: /openmrs
  hosts:
    - host: openmrs.livinggoods.net
      paths:
       - /
  tls:
    - hosts:
      - openmrs.example.com
      secretName: openmrs-tls
mysql:
  root:
    password: Admin123
  db:
    user: openmrs
    password: Admin123
    name: openmrs
  replication:
    enabled: false
```
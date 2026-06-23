Running with gitlab-runner 17.9.2 (14c5775c)
  on dc_runner_55 Wz1c0kGRX, system ID: s_300da9d7588c
Resolving secrets
Preparing the "docker" executor
00:02
Using Docker executor with image artifactory.jfrog.sbi:443/itepaypg-sbiepay2-docker-local/custom-ci/rhelgit-oc-helm:1.0 ...
Using helper image:  artifactory.jfrog.sbi:443/dso-base-image/gitlab-runner-helper/gitlab-runner-helper:x86_64-v17.9.2  (overridden, default would be  registry.gitlab.com/gitlab-org/gitlab-runner/gitlab-runner-helper:x86_64-v17.9.2 )
Authenticating with credentials from $DOCKER_AUTH_CONFIG
Pulling docker image artifactory.jfrog.sbi:443/dso-base-image/gitlab-runner-helper/gitlab-runner-helper:x86_64-v17.9.2 ...
Using docker image sha256:4511164d4b592f8cb69c7ffe5cb2df5d1909c1e7924081721495a61e4b03f657 for artifactory.jfrog.sbi:443/dso-base-image/gitlab-runner-helper/gitlab-runner-helper:x86_64-v17.9.2 with digest artifactory.jfrog.sbi:443/dso-base-image/gitlab-runner-helper/gitlab-runner-helper@sha256:47dfd72820e9c3b93c84dcdc2e689ba1236880b4c01de59bbf0a26f9e72b2a35 ...
Using helper image:  artifactory.jfrog.sbi:443/dso-base-image/gitlab-runner-helper/gitlab-runner-helper:x86_64-v17.9.2  (overridden, default would be  registry.gitlab.com/gitlab-org/gitlab-runner/gitlab-runner-helper:x86_64-v17.9.2 )
Using docker image sha256:4511164d4b592f8cb69c7ffe5cb2df5d1909c1e7924081721495a61e4b03f657 for artifactory.jfrog.sbi:443/dso-base-image/gitlab-runner-helper/gitlab-runner-helper:x86_64-v17.9.2 with digest artifactory.jfrog.sbi:443/dso-base-image/gitlab-runner-helper/gitlab-runner-helper@sha256:47dfd72820e9c3b93c84dcdc2e689ba1236880b4c01de59bbf0a26f9e72b2a35 ...
Authenticating with credentials from $DOCKER_AUTH_CONFIG
Pulling docker image artifactory.jfrog.sbi:443/itepaypg-sbiepay2-docker-local/custom-ci/rhelgit-oc-helm:1.0 ...
Using docker image sha256:db06a3e48eaa4339a92d53be57a37a699ba188fed41652c6c3c6c07bb86cb6f0 for artifactory.jfrog.sbi:443/itepaypg-sbiepay2-docker-local/custom-ci/rhelgit-oc-helm:1.0 with digest artifactory.jfrog.sbi:443/itepaypg-sbiepay2-docker-local/custom-ci/rhelgit-oc-helm@sha256:65605f26e63ed7624e4711cdd933caf6577f9d309f240d48a295f1d2c790c4d9 ...
Preparing environment
00:00
Using helper image:  artifactory.jfrog.sbi:443/dso-base-image/gitlab-runner-helper/gitlab-runner-helper:x86_64-v17.9.2  (overridden, default would be  registry.gitlab.com/gitlab-org/gitlab-runner/gitlab-runner-helper:x86_64-v17.9.2 )
Using docker image sha256:4511164d4b592f8cb69c7ffe5cb2df5d1909c1e7924081721495a61e4b03f657 for artifactory.jfrog.sbi:443/dso-base-image/gitlab-runner-helper/gitlab-runner-helper:x86_64-v17.9.2 with digest artifactory.jfrog.sbi:443/dso-base-image/gitlab-runner-helper/gitlab-runner-helper@sha256:47dfd72820e9c3b93c84dcdc2e689ba1236880b4c01de59bbf0a26f9e72b2a35 ...
Running on runner-wz1c0kgrx-project-2838-concurrent-0 via PE3DSOPGrunners4...
Getting source from Git repository
00:13
Fetching changes...
Initialized empty Git repository in /builds/itepaypg-sbiepay2/infra/devops/deployment/.git/
Created fresh repository.
Checking out c6b7813d as detached HEAD (ref is main)...
Skipping Git submodules setup
Executing "step_script" stage of the job script
00:05
Using docker image sha256:db06a3e48eaa4339a92d53be57a37a699ba188fed41652c6c3c6c07bb86cb6f0 for artifactory.jfrog.sbi:443/itepaypg-sbiepay2-docker-local/custom-ci/rhelgit-oc-helm:1.0 with digest artifactory.jfrog.sbi:443/itepaypg-sbiepay2-docker-local/custom-ci/rhelgit-oc-helm@sha256:65605f26e63ed7624e4711cdd933caf6577f9d309f240d48a295f1d2c790c4d9 ...
$ set -eo pipefail # collapsed multi-line command
Service=epaylite-bridge-mis-service | ENV=dev | VERSION=
$ echo "RELEASE_NAME=$RELEASE_NAME" # collapsed multi-line command
RELEASE_NAME=epaylite-bridge-mis-service
SERVICE_NAME=epaylite-bridge-mis-service
ENV=dev
WARNING: Using insecure TLS client config. Setting this option is not supported!
Logged into "https://api.dev.sbiepay.sbi:6443" as "system:serviceaccount:dev-sbiepay:cicd-sa" using the token provided.
You have access to 217 projects, the list has been suppressed. You can list all projects with 'oc projects'
Using project "default".
Welcome! See 'oc help' to get started.
Synced dev/charts/epaylite-bridge-mis-service/values.yaml to origin/main
Deploying with image tag from values.yaml: mr-feature-dev-bridge-mis-develop-2-e988e4b1
Release "epaylite-bridge-mis-service" does not exist. Installing it now.
Error: 1 error occurred:
	* Namespace "dev-" is invalid: [metadata.name: Invalid value: "dev-": a lowercase RFC 1123 label must consist of lower case alphanumeric characters or '-', and must start and end with an alphanumeric character (e.g. 'my-name',  or '123-abc', regex used for validation is '[a-z0-9]([-a-z0-9]*[a-z0-9])?'), metadata.labels: Invalid value: "dev-": a valid label must be an empty string or consist of alphanumeric characters, '-', '_' or '.', and must start and end with an alphanumeric character (e.g. 'MyValue',  or 'my_value',  or '12345', regex used for validation is '(([A-Za-z0-9][-A-Za-z0-9_.]*)?[A-Za-z0-9])?')]
Cleaning up project directory and file based variables
00:00
ERROR: Job failed: exit code 1



# Default values for epaytransactionservice.
# This is a YAML-formatted file.
# Declare variables to be passed into your templates.

replicaCount: 1

virtualService:
  enabled: true

image:
  repository: "artifactory.jfrog.sbi:443/itepaypg-sbiepay2-docker-local/app/backend/epaylite-bridge-mis-service"
  pullPolicy: IfNotPresent
  # Overrides the image tag whose default is the chart appVersion.
  ##tag: "v-develop-20260407095033"
  tag: "mr-feature-dev-bridge-mis-develop-2-e988e4b1"


imagePullSecrets:
  - name: jfrog-pull-secret
nameOverride: ""
fullnameOverride: ""

serviceAccount: {}

podAnnotations: {}
podLabels: {}

#podSecurityContext: {}

podSecurityContext: 
  runAsNonRoot: true
  #fsGroup: 1001190000
  seccompProfile:
    type: RuntimeDefault
  # runAsUser: 0
  # fsGroup: 0
  # fsGroupChangePolicy: Always

securityContext: 
  runAsNonRoot: true
  allowPrivilegeEscalation: false
  capabilities:
    drop: ["ALL"]
  seccompProfile:
    type: RuntimeDefault

service:
  type: ClusterIP
  port: 8099

namespace: dev-bridge
gatewayName: dev-istio-system/dev-gateway
virtualServiceName: epaylitebridge-misservice
serviceName: epaylitebridge-misservice
prefixName: /epaylite-bridge-mis-service
servicePort: 8099
gatewayPort: 80
gatewayHost: "dev.epay.sbi"

  #  - secretName: chart-example-tls
  #    hosts:
  #      - chart-example.local
##
  
configMap: 
  additionalLabels:
    # ...
  annotations:
    # ...
  nameSuffix: "config"

resources:
  requests:
    memory: "2Gi"
    cpu: "2"
  limits:
    memory: "3Gi"
    cpu: "3"
ingress: {}
#readinessProbe: {}
#livenessProbe: {}

# Enable health probes for proper HPA functioning
readinessProbe:
  enabled: false
  httpGet:
    path: "/api/bridge/v1/actuator/health/readiness"
    port: 8099
    scheme: HTTP
  initialDelaySeconds: 60
  periodSeconds: 10

livenessProbe:
  enabled: false
  httpGet:
    path: "/api/bridge/v1/actuator/health/liveness"
    port: 8099
    scheme: HTTP
  initialDelaySeconds: 30
  periodSeconds: 10

# HPA Configuration
hpa:
  enabled: true
  minReplicas: 2
  maxReplicas: 30
  targetCPUUtilizationPercentage: 70
  targetMemoryUtilizationPercentage: 80
  behavior:
    scaleUp:
      stabilizationWindowSeconds: 60
      policies:
      - type: Percent
        value: 50
        periodSeconds: 60
    scaleDown:
      stabilizationWindowSeconds: 300
      policies:
      - type: Percent
        value: 10
        periodSeconds: 60
autoscaling:
  enabled: true
# Additional volumes on the output Deployment definition.
volumes: []
# Additional volumeMounts on the output Deployment definition.
volumeMounts: []
# - name: foo
#   mountPath: "/etc/foo"
#   readOnly: true#


# customCertsTransaction:
#   enabled: true
#   secretName: custom-certs-transaction
#   mountPaths: /etc/custom-certs-transaction
# initContainer:
#   enabled: true
#   image: "registry.dev.sbiepay.sbi:8443/ubi9/openjdk-21:1.23-6.1756793462"
#   truststorePassword: "changeit"

nodeSelector: {}

tolerations: []

affinity: {}
#

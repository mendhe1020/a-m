Chart.yaml
1.14 KiB
apiVersion: v2
name: epaylite-bridge-txn-service
description: A Helm chart for Kubernetes
# A chart can be either an 'application' or a 'library' chart.
#
# Application charts are a collection of templates that can be packaged into versioned archives
# to be deployed.
#
# Library charts provide useful utilities or functions for the chart developer. They're included as
# a dependency of application charts to inject those utilities and functions into the rendering
# pipeline. Library charts do not define any templates and therefore cannot be deployed.
type: application
# This is the chart version. This version number should be incremented each time you make changes
# to the chart and its templates, including the app version.
# Versions are expected to follow Semantic Versioning (https://semver.org/)
version: 0.1.0
# This is the version number of the application being deployed. This version number should be
# incremented each time you make changes to the application. Versions are not expected to
# follow Semantic Versioning. They should reflect the version the application is using.
# It is recommended to use it with quotes.
appVersion: "1.16.0"

values.yaml
2.86 KiB
# Default values for epaytransactionservice.
# This is a YAML-formatted file.
# Declare variables to be passed into your templates.
replicaCount: 1
virtualService:
  enabled: true
image:
  repository: "artifactory.jfrog.sbi:443/itepaypg-sbiepay2-docker-local/app/backend/{{ .Chart.Name }}"
  pullPolicy: IfNotPresent
  # Overrides the image tag whose default is the chart appVersion.
  #tag: "v-develop-20260407095033"
  tag: ""
imagePullSecrets: []
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
namespace: epaylite-bridge-txt
gatewayName: dev-istio-system/dev-gateway
virtualServiceName: epaylitebridge-txnservice
serviceName: epaylitebridge-txnservice
prefixName: /epaylite-bridge-txn-service
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


deployment.yaml
Ghanshyam  Baboo's avatar
Update 20 files
Ghanshyam Baboo authored 13 minutes ago
54d48b9d
 Code owners
Assign users and groups as approvers for specific file changes. Learn more.
deployment.yaml
4.42 KiB
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ include "Epay_Bridge_Service.fullname" . }}
  labels:
    {{- include "Epay_Bridge_Service.labels" . | nindent 4 }}
spec:
  {{- if not .Values.autoscaling.enabled }}
  replicas: {{ .Values.replicaCount }}
  {{- end }}
  selector:
    matchLabels:
      {{- include "Epay_Bridge_Service.selectorLabels" . | nindent 6 }}
  template:
    metadata:
      {{- with .Values.podAnnotations }}
      annotations:
        {{- toYaml . | nindent 8 }}
      {{- end }}
      labels:
        {{- include "Epay_Bridge_Service.labels" . | nindent 8 }}
        {{- with .Values.podLabels }}
        {{- toYaml . | nindent 8 }}
        {{- end }}
    spec:
      {{- with .Values.imagePullSecrets }}
      imagePullSecrets:
        {{- toYaml . | nindent 8 }}
      {{- end }}
      serviceAccountName: {{ include "Epay_Bridge_Service.serviceAccountName" . }}
      securityContext:
        {{- toYaml .Values.podSecurityContext | nindent 8 }}
      containers:
        - name: {{ .Chart.Name }}
          securityContext:
            {{- toYaml .Values.securityContext | nindent 12 }}
          image: "{{ tpl .Values.image.repository $ }}:{{ .Values.image.tag | default .Chart.AppVersion }}"
          imagePullPolicy: {{ .Values.image.pullPolicy }}
          ports:
            - name: http
              containerPort: {{ .Values.service.port }}
              protocol: TCP
          env:
          - name: JAVA_TOOL_OPTIONS
            value: "-Djavax.net.ssl.trustStore=/cacerts/data/cacerts -Djavax.net.ssl.trustStorePassword=changeit"
          - name: SPRING_CONFIG_LOCATION
            value: file:/opt/gradle/
          - name: DB_USERNAME
            valueFrom:
              secretKeyRef:
                name: my-secrets-transaction-service-secret
                key: DB_USERNAME
          - name: DB_PASSWORD
            valueFrom:
              secretKeyRef:
                name: my-secrets-transaction-service-secret
                key: DB_PASSWORD
          # - name: external.api.sms.gateway.user
          #   valueFrom:
          #     secretKeyRef:
          #       key: external.api.sms.gateway.user
          #       name: my-secrets-transaction-service-secret
          # - name: external.api.sms.gateway.password
          #   valueFrom:
          #     secretKeyRef:
          #       key: external.api.sms.gateway.password
          #       name: my-secrets-transaction-service-secret
          # - name: spring.mail.password
          #   valueFrom:
          #     secretKeyRef:
          #       key: spring.mail.password
          #       name: my-secrets-transaction-service-secret
          # - name: spring.mail.username
          #   valueFrom:
          #     secretKeyRef:
          #       key: spring.mail.username
          #       name: my-secrets-transaction-service-secret
          {{- if .Values.readinessProbe.enabled }}
          readinessProbe:
            httpGet:
              # path: {{ .Values.prefixName }}/actuator/health
              # port: {{ .Values.service.port }}
              path: {{ .Values.readinessProbe.httpGet.path }}
              port: {{ .Values.readinessProbe.httpGet.port }}
            initialDelaySeconds: 30
            periodSeconds: 10
            timeoutSeconds: 5
            failureThreshold: 3
            successThreshold: 1
          {{- end }}
          {{- if .Values.livenessProbe.enabled }}
          livenessProbe:
            httpGet:
              # path: {{ .Values.prefixName }}/actuator/health
              # port: {{ .Values.service.port }}
              path: {{ .Values.livenessProbe.httpGet.path }}
              port: {{ .Values.livenessProbe.httpGet.port }}
            initialDelaySeconds: 60
            periodSeconds: 20
            timeoutSeconds: 10
            failureThreshold: 6
            successThreshold: 1
          {{- end }}
          envFrom:
            - secretRef:
                name: eiscert      
          volumeMounts:
            - name: config
              mountPath: "/opt/gradle"
            - name: dcms-txn-cert
              readOnly: true
              mountPath: /certs  
            - name: cacerts
              mountPath: /cacerts
              readOnly: true
                 
      volumes:
        - name: config
          configMap:
            name: epaybridge-config
        - name: dcms-txn-cert
          secret:
            secretName: eiscert
            defaultMode: 420
        - name: cacerts
          emptyDir: {}
          
#


update-tag
Failed
 Started 57 minutes ago by 
Ghanshyam Baboo
Search visible log output
Running with gitlab-runner 17.9.2 (14c5775c)
  on dc_runner_54 zFOGPH9mk, system ID: s_300da9d7588c
Resolving secrets
Preparing the "docker" executor
00:01
Using Docker executor with image artifactory.jfrog.sbi:443/itepaypg-sbiepay2-docker-local/custom-ci/rhelgit-oc-helm:1.0 ...
Using helper image:  artifactory.jfrog.sbi:443/dso-base-image/gitlab-runner-helper/gitlab-runner-helper:x86_64-v17.9.2  (overridden, default would be  registry.gitlab.com/gitlab-org/gitlab-runner/gitlab-runner-helper:x86_64-v17.9.2 )
Using locally found image version due to "if-not-present" pull policy
Using docker image sha256:4511164d4b592f8cb69c7ffe5cb2df5d1909c1e7924081721495a61e4b03f657 for artifactory.jfrog.sbi:443/dso-base-image/gitlab-runner-helper/gitlab-runner-helper:x86_64-v17.9.2 with digest artifactory.jfrog.sbi:443/dso-base-image/gitlab-runner-helper/gitlab-runner-helper@sha256:47dfd72820e9c3b93c84dcdc2e689ba1236880b4c01de59bbf0a26f9e72b2a35 ...
Using helper image:  artifactory.jfrog.sbi:443/dso-base-image/gitlab-runner-helper/gitlab-runner-helper:x86_64-v17.9.2  (overridden, default would be  registry.gitlab.com/gitlab-org/gitlab-runner/gitlab-runner-helper:x86_64-v17.9.2 )
Using docker image sha256:4511164d4b592f8cb69c7ffe5cb2df5d1909c1e7924081721495a61e4b03f657 for artifactory.jfrog.sbi:443/dso-base-image/gitlab-runner-helper/gitlab-runner-helper:x86_64-v17.9.2 with digest artifactory.jfrog.sbi:443/dso-base-image/gitlab-runner-helper/gitlab-runner-helper@sha256:47dfd72820e9c3b93c84dcdc2e689ba1236880b4c01de59bbf0a26f9e72b2a35 ...
Using locally found image version due to "if-not-present" pull policy
Using docker image sha256:db06a3e48eaa4339a92d53be57a37a699ba188fed41652c6c3c6c07bb86cb6f0 for artifactory.jfrog.sbi:443/itepaypg-sbiepay2-docker-local/custom-ci/rhelgit-oc-helm:1.0 with digest artifactory.jfrog.sbi:443/itepaypg-sbiepay2-docker-local/custom-ci/rhelgit-oc-helm@sha256:65605f26e63ed7624e4711cdd933caf6577f9d309f240d48a295f1d2c790c4d9 ...
Preparing environment
00:01
Using helper image:  artifactory.jfrog.sbi:443/dso-base-image/gitlab-runner-helper/gitlab-runner-helper:x86_64-v17.9.2  (overridden, default would be  registry.gitlab.com/gitlab-org/gitlab-runner/gitlab-runner-helper:x86_64-v17.9.2 )
Using docker image sha256:4511164d4b592f8cb69c7ffe5cb2df5d1909c1e7924081721495a61e4b03f657 for artifactory.jfrog.sbi:443/dso-base-image/gitlab-runner-helper/gitlab-runner-helper:x86_64-v17.9.2 with digest artifactory.jfrog.sbi:443/dso-base-image/gitlab-runner-helper/gitlab-runner-helper@sha256:47dfd72820e9c3b93c84dcdc2e689ba1236880b4c01de59bbf0a26f9e72b2a35 ...
Running on runner-zfogph9mk-project-2838-concurrent-0 via PE3DSOPGrunners3...
Getting source from Git repository
00:09
Fetching changes...
Initialized empty Git repository in /builds/itepaypg-sbiepay2/infra/devops/deployment/.git/
Created fresh repository.
Checking out d92a4fed as detached HEAD (ref is main)...
Skipping Git submodules setup
Executing "step_script" stage of the job script
00:01
Using docker image sha256:db06a3e48eaa4339a92d53be57a37a699ba188fed41652c6c3c6c07bb86cb6f0 for artifactory.jfrog.sbi:443/itepaypg-sbiepay2-docker-local/custom-ci/rhelgit-oc-helm:1.0 with digest artifactory.jfrog.sbi:443/itepaypg-sbiepay2-docker-local/custom-ci/rhelgit-oc-helm@sha256:65605f26e63ed7624e4711cdd933caf6577f9d309f240d48a295f1d2c790c4d9 ...
$ git config --global http.sslVerify false
$ git config --global --add safe.directory '*'
$ git config --global user.name "ci"
$ git config --global user.email "ci.cedge@sbi.co.in"
$ git remote set-url origin "https://oauth2:$GITLAB_CI_DEPLOY_TOKEN@gitlab.sbi/$CI_PROJECT_PATH.git"
$ set -eo pipefail # collapsed multi-line command
========== DEBUG ==========
SERVICE_NAME=epaylite-bridge-txn-service
ENV=dev
VERSION=dev-fb09d97c
CHART_NAME=$CHART_NAME
RELEASE_NAME=epaybridgetxn
===========================
Updating dev/charts/epaylite-bridge-txn-service/values.yaml → image tag: dev-fb09d97c
SERVICE_PATH=dev/charts/epaylite-bridge-txn-service
ls: cannot access 'dev/charts/epaylite-bridge-txn-service': No such file or directory
Cleaning up project directory and file based variables
00:00
ERROR: Job failed: exit code 1

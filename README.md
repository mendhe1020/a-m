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

namespace: epaylite-bridge-mis
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

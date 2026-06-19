####Default values for Epay_Payment_Service
replicaCount: 1

image:
  repository: artifactory.jfrog.sbi:443/itepaypg-sbiepay2-docker-local/app/backend/epay_payment_service

  pullPolicy: IfNotPresent
  tag: "1.2.0-rc.1"
#
imagePullSecrets: 
  - name: jfrog-pull-secret
nameOverride: ""
fullnameOverride: ""
serviceAccount: {}

podAnnotations: {}
podLabels: {}

configMap: 
  additionalLabels:
    # ...
  annotations:
    # ...
  nameSuffix: "config"


service:
  type: ClusterIP
  port: 9093

namespace: sit-transaction
gatewayName: sit-istio-system/sit-gateway
virtualServiceName: payment-paymentservice
prefixName: /api/payments/v1
serviceName: payment-paymentservice
servicePort: 9093
gatewayPort: 80
gatewayHost: "sit.epay.sbi"



ingress: {}
# readinessProbe: {}
# livenessProbe: {}

# Enable health probes for proper HPA functioning
readinessProbe:
  enabled: true
  httpGet:
    path: "/api/payments/v1/actuator/health/readiness"
    port: 9093
    scheme: HTTP
  initialDelaySeconds: 60
  periodSeconds: 10

livenessProbe:
  enabled: true
  httpGet:
    path: "/api/payments/v1/actuator/health/liveness"
    port: 9093
    scheme: HTTP
  initialDelaySeconds: 30
  periodSeconds: 10

autoscaling:
  enabled: false
  minReplicas: 1
  maxReplicas: 100
  targetCPUUtilizationPercentage: 80

securityContext: {}
podSecurityContext: {}

volumes: []
volumeMounts: []

nodeSelector: {}
tolerations: []
affinity: {}





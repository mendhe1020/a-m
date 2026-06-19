[osuser@bastion ~]$ oc get deployment comm-commsservice -o wide
NAME                READY   UP-TO-DATE   AVAILABLE   AGE    CONTAINERS     IMAGES                                                                                                       SELECTOR
comm-commsservice   0/1     1            0           101d   commsservice   artifactory.jfrog.sbi:443/itepaypg-sbiepay2-docker-local/app/backend/epay_communication_service:1.2.0-rc.1   app.kubernetes.io/instance=comm,app.kubernetes.io/name=commsservice
[osuser@bastion ~]$ oc get deployment comm-commsservice -o yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  annotations:
    deployment.kubernetes.io/revision: "8"
    meta.helm.sh/release-name: comm
    meta.helm.sh/release-namespace: sit-communication
  creationTimestamp: "2026-03-10T05:42:40Z"
  generation: 8
  labels:
    app.kubernetes.io/instance: comm
    app.kubernetes.io/managed-by: Helm
    app.kubernetes.io/name: commsservice
    app.kubernetes.io/version: 1.16.0
    helm.sh/chart: commsservice-0.1.0
  name: comm-commsservice
  namespace: sit-communication
  resourceVersion: "825185419"
  uid: 2b97d40e-7de3-4ef3-9acd-17962b1bbaa9
spec:
  progressDeadlineSeconds: 600
  replicas: 1
  revisionHistoryLimit: 10
  selector:
    matchLabels:
      app.kubernetes.io/instance: comm
      app.kubernetes.io/name: commsservice
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
  template:
    metadata:
      creationTimestamp: null
      labels:
        app.kubernetes.io/instance: comm
        app.kubernetes.io/managed-by: Helm
        app.kubernetes.io/name: commsservice
        app.kubernetes.io/version: 1.16.0
        helm.sh/chart: commsservice-0.1.0
    spec:
      containers:
      - env:
        - name: SPRING_CONFIG_LOCATION
          value: file:/opt/gradle/
        image: artifactory.jfrog.sbi:443/itepaypg-sbiepay2-docker-local/app/backend/epay_communication_service:1.2.0-rc.1
        imagePullPolicy: IfNotPresent
        name: commsservice
        ports:
        - containerPort: 9098
          name: http
          protocol: TCP
        resources: {}
        securityContext: {}
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
        volumeMounts:
        - mountPath: /opt/gradle
          name: config
      dnsPolicy: ClusterFirst
      imagePullSecrets:
      - name: jfrog-pull-secret
      restartPolicy: Always
      schedulerName: default-scheduler
      securityContext: {}
      serviceAccount: default
      serviceAccountName: default
      terminationGracePeriodSeconds: 30
      volumes:
      - configMap:
          defaultMode: 420
          name: comm-config
        name: config
status:
  conditions:
  - lastTransitionTime: "2026-03-10T05:42:40Z"
    lastUpdateTime: "2026-06-18T14:48:47Z"
    message: ReplicaSet "comm-commsservice-5674777bcd" has successfully progressed.
    reason: NewReplicaSetAvailable
    status: "True"
    type: Progressing
  - lastTransitionTime: "2026-06-19T05:58:26Z"
    lastUpdateTime: "2026-06-19T05:58:26Z"
    message: Deployment does not have minimum availability.
    reason: MinimumReplicasUnavailable
    status: "False"
    type: Available
  observedGeneration: 8
  replicas: 1
  unavailableReplicas: 1
  updatedReplicas: 1
[osuser@bastion ~]$

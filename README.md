# a-m
[osuser@bastion ~]$ oc get po -n dev-admin
NAME                                               READY   STATUS             RESTARTS         AGE
admin-adminservice-5c4bdb8f7f-krqmn                2/2     Running            0                40h
admin-adminservice-6b74dc76f7-g7j4c                1/2     CrashLoopBackOff   128 (117s ago)   10h
admin-portal-adminportalservice-67fd5b89f6-bc44k   2/2     Running            0                2d16h
[osuser@bastion ~]$ oc discribe po admin-adminservice-6b74dc76f7-g7j4c -n  dev-admin
error: unknown command "discribe" for "oc"

Did you mean this?
        describe
[osuser@bastion ~]$ oc describe po admin-adminservice-6b74dc76f7-g7j4c -n  dev-admin
Name:             admin-adminservice-6b74dc76f7-g7j4c
Namespace:        dev-admin
Priority:         0
Service Account:  default
Node:             worker1.dev.sbiepay.sbi/10.177.142.141
Start Time:       Sun, 07 Jun 2026 23:44:46 +0530
Labels:           app.kubernetes.io/instance=admin
                  app.kubernetes.io/managed-by=Helm
                  app.kubernetes.io/name=adminservice
                  app.kubernetes.io/version=1.16.0
                  helm.sh/chart=adminservice-0.1.0
                  pod-template-hash=6b74dc76f7
                  security.istio.io/tlsMode=istio
                  service.istio.io/canonical-name=adminservice
                  service.istio.io/canonical-revision=1.16.0
Annotations:      checksum/configmap: 62b51fbb1cf25e9ec33af936970aab0daded50c7659857bd982d69b05f20e5ca
                  istio.io/rev: dev-istio
                  k8s.ovn.org/pod-networks:
                    {"default":{"ip_addresses":["172.16.8.224/23"],"mac_address":"0a:58:ac:10:08:e0","gateway_ips":["172.16.8.1"],"routes":[{"dest":"172.16.0....
                  k8s.v1.cni.cncf.io/network-status:
                    [{
                        "name": "ovn-kubernetes",
                        "interface": "eth0",
                        "ips": [
                            "172.16.8.224"
                        ],
                        "mac": "0a:58:ac:10:08:e0",
                        "default": true,
                        "dns": {}
                    }]
                  k8s.v1.cni.cncf.io/networks: default/istio-cni
                  kubectl.kubernetes.io/default-container: adminservice
                  kubectl.kubernetes.io/default-logs-container: adminservice
                  kubectl.kubernetes.io/restartedAt: 2026-05-14T14:30:31+05:30
                  openshift.io/scc: restricted-v2
                  openshift.openshift.io/restartedAt: 2026-05-14T08:44:04.781Z
                  prometheus.io/path: /stats/prometheus
                  prometheus.io/port: 15020
                  prometheus.io/scrape: true
                  seccomp.security.alpha.kubernetes.io/pod: runtime/default
                  sidecar.istio.io/interceptionMode: REDIRECT
                  sidecar.istio.io/status:
                    {"initContainers":["istio-validation"],"containers":["istio-proxy"],"volumes":["workload-socket","credential-socket","workload-certs","ist...
                  traffic.sidecar.istio.io/excludeInboundPorts: 15020
                  traffic.sidecar.istio.io/includeInboundPorts: *
                  traffic.sidecar.istio.io/includeOutboundIPRanges: *
Status:           Running
SeccompProfile:   RuntimeDefault
IP:               172.16.8.224
IPs:
  IP:           172.16.8.224
Controlled By:  ReplicaSet/admin-adminservice-6b74dc76f7
Init Containers:
  istio-validation:
    Container ID:  cri-o://bd4776379bed3b70c45dacc4ab35d095b328304df6ecea9ae6ec8a9ebe5ac4ff
    Image:         registry.redhat.io/openshift-service-mesh/istio-proxyv2-rhel9@sha256:1306526a12590e284b1170cfccdf91388520360e2d7afab0d7f57111c4f28662
    Image ID:      registry.redhat.io/openshift-service-mesh/istio-proxyv2-rhel9@sha256:1306526a12590e284b1170cfccdf91388520360e2d7afab0d7f57111c4f28662
    Port:          <none>
    Host Port:     <none>
    Args:
      istio-iptables
      -p
      15001
      -z
      15006
      -u
      1001279999
      -m
      REDIRECT
      -i
      *
      -x

      -b
      *
      -d
      15090,15021,15020
      --log_output_level=default:info
      --run-validation
      --skip-rule-apply
    State:          Terminated
      Reason:       Completed
      Exit Code:    0
      Started:      Sun, 07 Jun 2026 23:44:48 +0530
      Finished:     Sun, 07 Jun 2026 23:44:48 +0530
    Ready:          True
    Restart Count:  0
    Limits:
      cpu:     2
      memory:  1Gi
    Requests:
      cpu:        100m
      memory:     128Mi
    Environment:  <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-dt88s (ro)
Containers:
  adminservice:
    Container ID:   cri-o://c36d16b592ec0ef7d2b394c4c6eae9116695b436248073f362bdb607b12d814c
    Image:          artifactory.jfrog.sbi:443/itepaypg-sbiepay2-docker-local/app/backend/epay_admin_service:mr-feature-epay-250-add-merchant-endpoint-develop-354-e07a9a99
    Image ID:       artifactory.jfrog.sbi:443/itepaypg-sbiepay2-docker-local/app/backend/epay_admin_service@sha256:5770980386f216db675d92191e679b56180cab1fe9e5f89e1630578f2c2b53de
    Port:           9094/TCP
    Host Port:      0/TCP
    State:          Waiting
      Reason:       CrashLoopBackOff
    Last State:     Terminated
      Reason:       Error
      Exit Code:    1
      Started:      Mon, 08 Jun 2026 10:36:30 +0530
      Finished:     Mon, 08 Jun 2026 10:36:42 +0530
    Ready:          False
    Restart Count:  128
    Liveness:       http-get http://:15020/app-health/adminservice/livez delay=30s timeout=10s period=20s #success=1 #failure=6
    Readiness:      http-get http://:15020/app-health/adminservice/readyz delay=60s timeout=5s period=10s #success=1 #failure=3
    Environment:
      SPRING_CONFIG_LOCATION:  file:/opt/gradle/
      DB_USERNAME:             <set to the key 'DB_USERNAME' in secret 'my-secrets-admin-service-secret'>  Optional: false
      DB_PASSWORD:             <set to the key 'DB_PASSWORD' in secret 'my-secrets-admin-service-secret'>  Optional: false
    Mounts:
      /certs from payment-upivpaqr-cert (ro)
      /opt/gradle from config (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-dt88s (ro)
  istio-proxy:
    Container ID:  cri-o://7356dfa7dcda934322a08a73adc930acc3e54cc85f0de4061dd6d4f89a533871
    Image:         registry.redhat.io/openshift-service-mesh/istio-proxyv2-rhel9@sha256:1306526a12590e284b1170cfccdf91388520360e2d7afab0d7f57111c4f28662
    Image ID:      registry.redhat.io/openshift-service-mesh/istio-proxyv2-rhel9@sha256:1306526a12590e284b1170cfccdf91388520360e2d7afab0d7f57111c4f28662
    Port:          15090/TCP
    Host Port:     0/TCP
    Args:
      proxy
      sidecar
      --domain
      $(POD_NAMESPACE).svc.cluster.local
      --proxyLogLevel=warning
      --proxyComponentLogLevel=misc:error
      --log_output_level=default:info
    State:          Running
      Started:      Sun, 07 Jun 2026 23:44:51 +0530
    Ready:          True
    Restart Count:  0
    Limits:
      cpu:     2
      memory:  1Gi
    Requests:
      cpu:      100m
      memory:   128Mi
    Readiness:  http-get http://:15021/healthz/ready delay=0s timeout=3s period=15s #success=1 #failure=4
    Startup:    http-get http://:15021/healthz/ready delay=0s timeout=3s period=1s #success=1 #failure=600
    Environment:
      PILOT_CERT_PROVIDER:           istiod
      CA_ADDR:                       istiod-dev-istio.dev-istio-system.svc:15012
      POD_NAME:                      admin-adminservice-6b74dc76f7-g7j4c (v1:metadata.name)
      POD_NAMESPACE:                 dev-admin (v1:metadata.namespace)
      INSTANCE_IP:                    (v1:status.podIP)
      SERVICE_ACCOUNT:                (v1:spec.serviceAccountName)
      HOST_IP:                        (v1:status.hostIP)
      ISTIO_CPU_LIMIT:               2 (limits.cpu)
      PROXY_CONFIG:                  {"discoveryAddress":"istiod-dev-istio.dev-istio-system.svc:15012"}

      ISTIO_META_POD_PORTS:          [
                                         {"name":"http","containerPort":9094,"protocol":"TCP"}
                                     ]
      ISTIO_META_APP_CONTAINERS:     adminservice
      GOMEMLIMIT:                    1073741824 (limits.memory)
      GOMAXPROCS:                    2 (limits.cpu)
      ISTIO_META_CLUSTER_ID:         Kubernetes
      ISTIO_META_NODE_NAME:           (v1:spec.nodeName)
      ISTIO_META_INTERCEPTION_MODE:  REDIRECT
      ISTIO_META_WORKLOAD_NAME:      admin-adminservice
      ISTIO_META_OWNER:              kubernetes://apis/apps/v1/namespaces/dev-admin/deployments/admin-adminservice
      ISTIO_META_MESH_ID:            cluster.local
      TRUST_DOMAIN:                  cluster.local
      ISTIO_KUBE_APP_PROBERS:        {"/app-health/adminservice/livez":{"httpGet":{"path":"/api/admin/v1/actuator/health/liveness","port":9094,"scheme":"HTTP"},"timeoutSeconds":10},"/app-health/adminservice/readyz":{"httpGet":{"path":"/api/admin/v1/actuator/health/readiness","port":9094,"scheme":"HTTP"},"timeoutSeconds":5}}
    Mounts:
      /etc/istio/pod from istio-podinfo (rw)
      /etc/istio/proxy from istio-envoy (rw)
      /var/lib/istio/data from istio-data (rw)
      /var/run/secrets/credential-uds from credential-socket (rw)
      /var/run/secrets/istio from istiod-ca-cert (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-dt88s (ro)
      /var/run/secrets/tokens from istio-token (rw)
      /var/run/secrets/workload-spiffe-credentials from workload-certs (rw)
      /var/run/secrets/workload-spiffe-uds from workload-socket (rw)
Conditions:
  Type                        Status
  PodReadyToStartContainers   True
  Initialized                 True
  Ready                       False
  ContainersReady             False
  PodScheduled                True
Volumes:
  workload-socket:
    Type:       EmptyDir (a temporary directory that shares a pod's lifetime)
    Medium:
    SizeLimit:  <unset>
  credential-socket:
    Type:       EmptyDir (a temporary directory that shares a pod's lifetime)
    Medium:
    SizeLimit:  <unset>
  workload-certs:
    Type:       EmptyDir (a temporary directory that shares a pod's lifetime)
    Medium:
    SizeLimit:  <unset>
  istio-envoy:
    Type:       EmptyDir (a temporary directory that shares a pod's lifetime)
    Medium:     Memory
    SizeLimit:  <unset>
  istio-data:
    Type:       EmptyDir (a temporary directory that shares a pod's lifetime)
    Medium:
    SizeLimit:  <unset>
  istio-podinfo:
    Type:  DownwardAPI (a volume populated by information about the pod)
    Items:
      metadata.labels -> labels
      metadata.annotations -> annotations
  istio-token:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  43200
  istiod-ca-cert:
    Type:      ConfigMap (a volume populated by a ConfigMap)
    Name:      istio-ca-root-cert
    Optional:  false
  config:
    Type:      ConfigMap (a volume populated by a ConfigMap)
    Name:      admin-config
    Optional:  false
  payment-upivpaqr-cert:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  payment-upivpaqr
    Optional:    false
  kube-api-access-dt88s:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
    ConfigMapName:           openshift-service-ca.crt
    ConfigMapOptional:       <nil>
QoS Class:                   Burstable
Node-Selectors:              env=dev
Tolerations:                 node.kubernetes.io/memory-pressure:NoSchedule op=Exists
                             node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason                 Age   From                                       Message
  ----    ------                 ----  ----                                       -------
  Normal  IPTablesUsageObserved  140m  openshift.io/iptables-deprecation-alerter  This pod appears to have created one or more iptables rules. IPTables is
deprecated and will no longer be available in RHEL 10 and later. You should
consider migrating to another API such as nftables or eBPF. See also
https://access.redhat.com/solutions/6739041

Example iptables rule seen in this pod:
-A PREROUTING -p tcp -j ISTIO_INBOUND
  Normal   Created  35m (x123 over 10h)     kubelet  Created container: adminservice
  Warning  BackOff  4m51s (x2971 over 10h)  kubelet  Back-off restarting failed container adminservice in pod admin-adminservice-6b74dc76f7-g7j4c_dev-admin(b127d416-3a1f-473b-89e8-dbb1da97d008)
  Normal   Pulled   3m23s (x128 over 10h)   kubelet  Container image "artifactory.jfrog.sbi:443/itepaypg-sbiepay2-docker-local/app/backend/epay_admin_service:mr-feature-epay-250-add-merchant-endpoint-develop-354-e07a9a99" already present on machine
[osuser@bastion ~]$

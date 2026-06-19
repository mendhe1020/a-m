[osuser@bastion ~]$ oc drecsib pod comm-commsservice-5674777bcd-fhbpr
error: unknown command "drecsib" for "oc"
[osuser@bastion ~]$  oc describe pod  comm-commsservice-5674777bcd-fhbpr
Name:             comm-commsservice-5674777bcd-fhbpr
Namespace:        sit-communication
Priority:         0
Service Account:  default
Node:             worker1.sit.epay.sbi/10.177.143.113
Start Time:       Thu, 18 Jun 2026 19:40:02 +0530
Labels:           app.kubernetes.io/instance=comm
                  app.kubernetes.io/managed-by=Helm
                  app.kubernetes.io/name=commsservice
                  app.kubernetes.io/version=1.16.0
                  helm.sh/chart=commsservice-0.1.0
                  pod-template-hash=5674777bcd
                  security.istio.io/tlsMode=istio
                  service.istio.io/canonical-name=commsservice
                  service.istio.io/canonical-revision=1.16.0
Annotations:      istio.io/rev: sit-istio
                  k8s.ovn.org/pod-networks:
                    {"default":{"ip_addresses":["172.16.24.86/23"],"mac_address":"0a:58:ac:10:18:56","gateway_ips":["172.16.24.1"],"routes":[{"dest":"172.16.0...
                  k8s.v1.cni.cncf.io/network-status:
                    [{
                        "name": "ovn-kubernetes",
                        "interface": "eth0",
                        "ips": [
                            "172.16.24.86"
                        ],
                        "mac": "0a:58:ac:10:18:56",
                        "default": true,
                        "dns": {}
                    }]
                  k8s.v1.cni.cncf.io/networks: default/istio-cni
                  kubectl.kubernetes.io/default-container: commsservice
                  kubectl.kubernetes.io/default-logs-container: commsservice
                  openshift.io/scc: restricted-v2
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
IP:               172.16.24.86
IPs:
  IP:           172.16.24.86
Controlled By:  ReplicaSet/comm-commsservice-5674777bcd
Init Containers:
  istio-validation:
    Container ID:  cri-o://af8d0d8346abd301b72603fe91af8aba92ee0236caf5fc594c723571e591cb5f
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
      1001309999
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
      Started:      Thu, 18 Jun 2026 19:40:03 +0530
      Finished:     Thu, 18 Jun 2026 19:40:03 +0530
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
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-72n9l (ro)
Containers:
  commsservice:
    Container ID:   cri-o://9550bd5a7bd74eccf65212e2f532b3af6c0d93c05619dec50f51076dcb94da53
    Image:          artifactory.jfrog.sbi:443/itepaypg-sbiepay2-docker-local/app/backend/epay_communication_service:1.2.0-rc.1
    Image ID:       artifactory.jfrog.sbi:443/itepaypg-sbiepay2-docker-local/app/backend/epay_communication_service@sha256:a56ed06e6034fa85449920946324d1c33e8f751dda5aeda1204f5f3a625be9e3
    Port:           9098/TCP
    Host Port:      0/TCP
    State:          Waiting
      Reason:       CrashLoopBackOff
    Last State:     Terminated
      Reason:       Error
      Exit Code:    1
      Started:      Fri, 19 Jun 2026 11:01:49 +0530
      Finished:     Fri, 19 Jun 2026 11:01:59 +0530
    Ready:          False
    Restart Count:  179
    Environment:
      SPRING_CONFIG_LOCATION:  file:/opt/gradle/
    Mounts:
      /opt/gradle from config (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-72n9l (ro)
  istio-proxy:
    Container ID:  cri-o://e7df5139f2260c85261a1c1914559bed8033ff9c86cba204818b40e6088ad565
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
      Started:      Thu, 18 Jun 2026 19:40:06 +0530
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
      CA_ADDR:                       istiod-sit-istio.sit-istio-system.svc:15012
      POD_NAME:                      comm-commsservice-5674777bcd-fhbpr (v1:metadata.name)
      POD_NAMESPACE:                 sit-communication (v1:metadata.namespace)
      INSTANCE_IP:                    (v1:status.podIP)
      SERVICE_ACCOUNT:                (v1:spec.serviceAccountName)
      HOST_IP:                        (v1:status.hostIP)
      ISTIO_CPU_LIMIT:               2 (limits.cpu)
      PROXY_CONFIG:                  {"discoveryAddress":"istiod-sit-istio.sit-istio-system.svc:15012"}

      ISTIO_META_POD_PORTS:          [
                                         {"name":"http","containerPort":9098,"protocol":"TCP"}
                                     ]
      ISTIO_META_APP_CONTAINERS:     commsservice
      GOMEMLIMIT:                    1073741824 (limits.memory)
      GOMAXPROCS:                    2 (limits.cpu)
      ISTIO_META_CLUSTER_ID:         Kubernetes
      ISTIO_META_NODE_NAME:           (v1:spec.nodeName)
      ISTIO_META_INTERCEPTION_MODE:  REDIRECT
      ISTIO_META_WORKLOAD_NAME:      comm-commsservice
      ISTIO_META_OWNER:              kubernetes://apis/apps/v1/namespaces/sit-communication/deployments/comm-commsservice
      ISTIO_META_MESH_ID:            cluster.local
      TRUST_DOMAIN:                  cluster.local
    Mounts:
      /etc/istio/pod from istio-podinfo (rw)
      /etc/istio/proxy from istio-envoy (rw)
      /var/lib/istio/data from istio-data (rw)
      /var/run/secrets/credential-uds from credential-socket (rw)
      /var/run/secrets/istio from istiod-ca-cert (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-72n9l (ro)
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
    Name:      comm-config
    Optional:  false
  kube-api-access-72n9l:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
    ConfigMapName:           openshift-service-ca.crt
    ConfigMapOptional:       <nil>
QoS Class:                   Burstable
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/memory-pressure:NoSchedule op=Exists
                             node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason                 Age   From                                       Message
  ----    ------                 ----  ----                                       -------
  Normal  IPTablesUsageObserved  27m   openshift.io/iptables-deprecation-alerter  This pod appears to have created one or more iptables rules. IPTables is
deprecated and will no longer be available in RHEL 10 and later. You should
consider migrating to another API such as nftables or eBPF. See also
https://access.redhat.com/solutions/6739041

Example iptables rule seen in this pod:
-A PREROUTING -p tcp -j ISTIO_INBOUND
  Normal   Created  32m (x174 over 15h)    kubelet  Created container: commsservice
  Warning  BackOff  112s (x4094 over 15h)  kubelet  Back-off restarting failed container commsservice in pod comm-commsservice-5674777bcd-fhbpr_sit-communication(61c51e1f-4258-4c40-89cc-947018003ba6)
  Normal   Pulled   34s (x179 over 15h)    kubelet  Container image "artifactory.jfrog.sbi:443/itepaypg-sbiepay2-docker-local/app/backend/epay_communication_service:1.2.0-rc.1" already present on machine
[osuser@bastion ~]$

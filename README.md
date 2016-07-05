# hello-kubernetes

```
$ curl -Lo minikube https://storage.googleapis.com/minikube/releases/v0.4.0/minikube-darwin-amd64 && chmod +x minikube

$ openssl sha1 minikube
SHA1(minikube)= 0e30c7936b427ceb8fc6e1994a7f5afa518d19fc

$ curl -O https://storage.googleapis.com/kubernetes-release/release/v1.3.0/bin/darwin/amd64/kubectl && chmod u+x kubectl

➜  kubernetes ls -alrt
total 252568
-rwxr-xr-x   1 peter_v  staff  73432848 Jul  5 18:43 minikube
-rwxr--r--   1 peter_v  staff  55849328 Jul  5 22:12 kubectl
...

$ export PATH=$PATH:/Users/peter_v/data/projects/kubernetes

$ minikube start # may take a few minutes
Starting local Kubernetes cluster...
Kubernetes is available at https://192.168.99.100:443.
Kubectl is now configured to use the cluster.

$ minikube docker-env
export DOCKER_TLS_VERIFY=1
export DOCKER_HOST=tcp://192.168.99.100:2376
export DOCKER_CERT_PATH=/Users/peter_v/.minikube/certs
# Run this command to configure your shell:
# eval $(minikube docker-env)

$ export DOCKER_TLS_VERIFY=1
export DOCKER_HOST=tcp://192.168.99.100:2376
$ export DOCKER_HOST=tcp://192.168.99.100:2376
$ export DOCKER_CERT_PATH=/Users/peter_v/.minikube/certs
$ kubernetes docker ps
CONTAINER ID        IMAGE                                                        COMMAND                  CREATED             STATUS              PORTS               NAMES
4fb1eed617b8        gcr.io/google_containers/kubernetes-dashboard-amd64:v1.1.0   "/dashboard --port=90"   33 seconds ago      Up 33 seconds                           k8s_kubernetes-dashboard.7c69d856_kubernetes-dashboard-pwrff_kube-system_ba94d54c-42ee-11e6-95d0-3abd9441c012_0e906c14
b137f10e0dd8        gcr.io/google_containers/pause-amd64:3.0                     "/pause"                 34 seconds ago      Up 34 seconds                           k8s_POD.2225036b_kubernetes-dashboard-pwrff_kube-system_ba94d54c-42ee-11e6-95d0-3abd9441c012_9deb9cd6
5564e766e768        gcr.io/google-containers/kube-addon-manager-amd64:v2         "/opt/kube-addons.sh"    36 seconds ago      Up 36 seconds                           k8s_kube-addon-manager.a1c58ca2_kube-addon-manager-127.0.0.1_kube-system_48abed82af93bb0b941173334110923f_19a8096b
809d33c8aa68        gcr.io/google_containers/pause-amd64:3.0                     "/pause"                 36 seconds ago      Up 36 seconds                           k8s_POD.d8dbe16c_kube-addon-manager-127.0.0.1_kube-system_48abed82af93bb0b941173334110923f_a8384c43

$ kubectl run hello-minikube --image=gcr.io/google_containers/echoserver:1.4 --hostport=8000 --port=8080 # takes a few minutes
deployment "hello-minikube" created
$ kubectl get pod
NAME                              READY     STATUS              RESTARTS   AGE
hello-minikube-3383150820-x72om   0/1       ContainerCreating   0          3s
$ kubectl get pod
NAME                              READY     STATUS              RESTARTS   AGE
hello-minikube-3383150820-x72om   0/1       ContainerCreating   0          5s

...

NAME                              READY     STATUS              RESTARTS   AGE
hello-minikube-3383150820-x72om   0/1       ContainerCreating   0          13s

...

$ kubectl get pod
NAME                              READY     STATUS    RESTARTS   AGE
hello-minikube-3383150820-x72om   1/1       Running   0          1m

$ kubectl describe pod hello-minikube-3383150820-x72om
Name:   hello-minikube-3383150820-x72om
Namespace:  default
Node:   127.0.0.1/127.0.0.1
Start Time: Tue, 05 Jul 2016 22:29:13 +0200
Labels:   pod-template-hash=3383150820
    run=hello-minikube
Status:   Running
IP:   172.17.0.3
Controllers:  ReplicaSet/hello-minikube-3383150820
Containers:
  hello-minikube:
    Container ID:   docker://a3b1c2cd0ddeb368ff9f5efb1a2ea314706afe8dbf53ba4826b7afb33ffe0c47
    Image:      gcr.io/google_containers/echoserver:1.4
    Image ID:     docker://sha256:a90209bb39e3d7b5fc9daf60c17044ea969aaca0333d672d8c7a34c7446e7ff7
    Port:     8080/TCP
    State:      Running
      Started:      Tue, 05 Jul 2016 22:29:46 +0200
    Ready:      True
    Restart Count:    0
    Environment Variables:  <none>
Conditions:
  Type    Status
  Initialized   True
  Ready   True
  PodScheduled  True
Volumes:
  default-token-atwxf:
    Type: Secret (a volume populated by a Secret)
    SecretName: default-token-atwxf
QoS Tier: BestEffort
Events:
  FirstSeen LastSeen  Count From      SubobjectPath     Type    Reason    Message
  --------- --------  ----- ----      -------------     --------  ------    -------
  1m    1m    1 {default-scheduler }          Normal    Scheduled Successfully assigned hello-minikube-3383150820-x72om to 127.0.0.1
  1m    1m    1 {kubelet 127.0.0.1} spec.containers{hello-minikube} Normal    Pulling   pulling image "gcr.io/google_containers/echoserver:1.4"
  1m    1m    1 {kubelet 127.0.0.1} spec.containers{hello-minikube} Normal    Pulled    Successfully pulled image "gcr.io/google_containers/echoserver:1.4"
  1m    1m    1 {kubelet 127.0.0.1} spec.containers{hello-minikube} Normal    Created   Created container with docker id a3b1c2cd0dde
  1m    1m    1 {kubelet 127.0.0.1} spec.containers{hello-minikube} Normal    Started   Started container with docker id a3b1c2cd0dde


➜  kubernetes nmap 192.168.99.100

Starting Nmap 7.12 ( https://nmap.org ) at 2016-07-05 22:32 CEST
Nmap scan report for 192.168.99.100
Host is up (0.0011s latency).
Not shown: 995 closed ports
PORT      STATE SERVICE
22/tcp    open  ssh
443/tcp   open  https
8000/tcp  open  http-alt
8081/tcp  open  blackice-icecap
30000/tcp open  unknown

Nmap done: 1 IP address (1 host up) scanned in 0.13 seconds
```

Result: Web site visible on 192.168.99.100:30000  :-)

To start and stop minikube use the following commands:
\ `minikube start`
\ `minikube stop`

Check if minikube started correspondly:
``kubectl version``

Pod: Wrapper of a container

See pods/services running:
``kubectl get all``

Running a port/service:
``kubectl apply -f file_name.yaml``

Getting cluster/minikube Ip address:
``minikube ip``

Get info about the specific pod:
``kubectl describe pod pod_name``

Connecting and running any command in any pod:
``kubectl exec pod_name -- ls``

Pods are not visible outside kubernetes cluster. That's why firstly we are trying to connect the port 80 from the inside of the pod.
Connecting pod and inside the pod we get a shell in the container which is in the pod and can test our access trying connect localhost from the shell:
    
    Connect to the pods shell interactively:
    kubectl -it exec pod_name sh

    Request the localhost in the pod:
    wget http:/localhost:80

Because of pods have the short life-cycle, they are recreated often,kubernetes has further consept called service which is long-running object.
a service have an IP address and stable port.
We can attach services to pods. With a service we can connect to the cluster and the service will find a suitable pod

Pod label: a key-value pair
In the runtime of kubernetes cluster the service look for any mathcing key-value pairs(labels) among the pods and where it finds it will select these pods. For this, selector must be privided in the service  with the same key-value pairs as in pods.

Service's types
ClusterIP: Only accessible inside the cluster.Can be used if the service is going to be internal service such as micro service. It makes it a kind of private service.
NodePort: We are exposing a port through the node. Node is in our case is kubernetes cluster.

*Note:* After any change in a pod/service yaml file for applying it  write running command again.

To deploy new version of the project(consider we need to use the image of the new version):
What Kubernetes would have to do is to stop the current pod and start the new one, to pull the new image. In other words, we are going to have some down time. We can do simple deployment with zero down time using labels.(exp: We give release:0 label to the old pod, create new pod with release:0.5 label. After running new pod, we add new release label to service selector.) 
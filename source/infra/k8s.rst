Kubernetes Udemy
================

Getting started
---------------

Minikube setup
^^^^^^^^^^^^^^

- Install kubectl
- Install minicube

**Installing kubectl**


LINUX:
``curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl``

``chmod +x ./kubectl``

``sudo mv ./kubectl /usr/local/bin/kubectl``

Testing kubectl:

``kubectl version``


**Installing minikube**

``curl -Lo minikube https://storage.googleapis.com/minikube/releases/v0.23.0/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/``

Start minikube:

``minikube start``

Deploy a sample Kubernetes "deployment" to your local minikube:

``kubectl run hello-minikube --image=gcr.io/google_containers/echoserver:1.4 --port=8080``

Expose this deployment to an external network:

``kubectl expose deployment hello-minikube  --type=NodePort``

List the "pods" of this deployment:

``kubectl get pod``

Access the sample service:

``curl $(minikube service hello-minikube --url)``

Delate the deployment:

``kubectl delete deployment hello-minikube``

Stop minikube:

``minikube stop``


Your first Kubernetes application
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

TOMCAT deployment

.. code-block:: bash

 apiVersion: apps/v1beta2
 kind: Deployment
 metadata:
   name: tomcat-deployment
 spec:
   selector:
     matchLabels:
       app: tomcat
   replicas: 1
   template:
     metadata:
       labels:
         app: tomcat
     spec:
       containers:
       - name: tomcat
         image: tomcat:9.0
         ports:
         - containerPort: 8080

``kubectl apply -f ./deployment.yaml``

Expose deployment as a service:

`` kubectl expose deployment tomcat-deployment --type=NodePort``

Get URL:

``minikube service tomcat-deployment --url``

Basic kubectl
^^^^^^^^^^^^^

List all pods in all namespaces and provides the pod name, how many instances of the pod are running & ready, its status, how many times they have restarted, and their age.

``kubectl get pods``

Sample Output:

.. code-block:: bash

 root@ubuntu:/home/luka/K8S# kubectl get pods
 NAME                                 READY     STATUS    RESTARTS   AGE
 tomcat-deployment-56ff5c79c5-rddwg   1/1       Running   0          8m
 root@ubuntu:/home/luka/K8S#


Describes detailed inforamtion about inforamtion about all pods or a specified pod (optional pod name argument):

``kubectl describe pod tomcat-deployment-56ff5c79c5-rddwg``


Sample Output:

.. code-block:: bash

 root@ubuntu:/home/luka/K8S# kubectl describe pods tomcat-deployment-56ff5c79c5-rddwg
 Name:           tomcat-deployment-56ff5c79c5-rddwg
 Namespace:      default
 Node:           minikube/192.168.99.100
 Start Time:     Thu, 26 Apr 2018 03:40:31 -0700
 Labels:         app=tomcat
                 pod-template-hash=1299173571
 Annotations:    <none>
 Status:         Running
 IP:             172.17.0.4
 Controlled By:  ReplicaSet/tomcat-deployment-56ff5c79c5
 Containers:
   tomcat:
     Container ID:   docker://aa41d780e14f4755537d9fdf82bb2ff3275e17b4bdcbd8ce92178e1170b14eee
     Image:          tomcat:9.0
     Image ID:       docker-pullable://tomcat@sha256:9cd9c3ca59af2a1b4e1958218dfbc68f599baa2b45d8821a5f655f95a04175cd
     Port:           8080/TCP
     Host Port:      0/TCP
     State:          Running
       Started:      Thu, 26 Apr 2018 03:41:57 -0700
     Ready:          True
     Restart Count:  0
     Environment:    <none>
     Mounts:
       /var/run/secrets/kubernetes.io/serviceaccount from default-token-vd246 (ro)
 Conditions:
   Type           Status
   Initialized    True
   Ready          True
   PodScheduled   True
 Volumes:
   default-token-vd246:
     Type:        Secret (a volume populated by a Secret)
     SecretName:  default-token-vd246
     Optional:    false
 QoS Class:       BestEffort
 Node-Selectors:  <none>
 Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                  node.kubernetes.io/unreachable:NoExecute for 300s
 Events:
   Type    Reason                 Age   From               Message
   ----    ------                 ----  ----               -------
   Normal  Scheduled              10m   default-scheduler  Successfully assigned tomcat-deployment-56ff5c79c5-rddwg to minikube
   Normal  SuccessfulMountVolume  10m   kubelet, minikube  MountVolume.SetUp succeeded for volume "default-token-vd246"
   Normal  Pulling                10m   kubelet, minikube  pulling image "tomcat:9.0"
   Normal  Pulled                 9m    kubelet, minikube  Successfully pulled image "tomcat:9.0"
   Normal  Created                9m    kubelet, minikube  Created container
   Normal  Started                9m    kubelet, minikube  Started container
 root@ubuntu:/home/luka/K8S#


Exposes a port (TCP or UDP) for a given deployment, pod, or other resource

``kubectl expose <type name> <identifier/name> [—port=external port] [—target-port=container-port [—type=service-type]``

Forwards one or more local ports to a pod (KUBECTL PORT-FORWARD):

``kubectl port-forward <pod name> [LOCAL_PORT:]REMOTE_PORT]``

Attaches to a proccess that is already running inside an existing container (KUBECTL ATTACH) :

``kubectl attach <pod name> -c <container>``


.. code-block:: bash

 root@ubuntu:/home/luka/K8S# kubectl attach tomcat-deployment-56ff5c79c5-rddwg
 Defaulting container name to tomcat.
 Use 'kubectl describe pod/tomcat-deployment-56ff5c79c5-rddwg -n default' to see all of the containers in this pod.
 If you don't see a command prompt, try pressing enter.

KUBECTL EXEC

``kubectl exec  [-it] <pod name> [-c CONTAINER] — COMMAND [args…]``

- Execute a command in a container
- -i option will pass stdin to the container
- -t option will specify stdin is a TTY

Sample Output:

.. code-block:: bash

 root@ubuntu:/home/luka/K8S# kubectl exec -it tomcat-deployment-56ff5c79c5-rddwg bash
 root@tomcat-deployment-56ff5c79c5-rddwg:/usr/local/tomcat# whoami
 root
 root@tomcat-deployment-56ff5c79c5-rddwg:/usr/local/tomcat#


KUBECTL LABEL PODS

Updates the labels on a resource

``kubectl label [—overwrite] <type> KEY_1=VAL_1 ….``

Sample Output:

.. code-block:: bash

 root@ubuntu:/home/luka/K8S# kubectl label pods tomcat-deployment-56ff5c79c5-rddwg healthy=false
 pod "tomcat-deployment-56ff5c79c5-rddwg" labeled
 root@ubuntu:/home/luka/K8S#

KUBECTL Running

``kubectl run <name> —image=image``

Run a particular image on the cluster

Sample Output:

.. code-block:: bash

 root@ubuntu:/home/luka/K8S# kubectl run hazelcast --image=hazelcast --port=5701
 deployment.apps "hazelcast" created
 root@ubuntu:/home/luka/K8S#



Kubernetes basic and core concepts
----------------------------------

Scaling Kubernetes
^^^^^^^^^^^^^^^^^^


Use tomcat deployment yaml file.

Scaling a practical example:

``kubectl scale --replicas=4 deployment/tomcat-deployment``

Sample Output:

.. code-block:: bash

 root@ubuntu:/home/luka/K8S# kubectl scale --replicas=4 deployment/tomcat-deployment
 deployment.extensions "tomcat-deployment" scaled
 root@ubuntu:/home/luka/K8S#

and after this use next command to verify:

.. code-block:: bash

 root@ubuntu:/home/luka/K8S# kubectl get deployments
 NAME                DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
 hazelcast           1         1         1            0           1h
 tomcat-deployment   4         4         4            4           1h
 root@ubuntu:/home/luka/K8S#


Updating the service

**Loadbalancer**

Previously, we defined a "NodePort" service for the Tomcat pod:

``kubectl expose deployment tomcat-deployment --type=NodePort``

Now, let's define a LoadBalancer service , instead:

``kubectl expose deployment tomcat-deployment --type=LoadBalancer --port=8080 --target-port=8080 --name tomcat-load-balancer``

Sample Output:

.. code-block:: bash

 root@ubuntu:/home/luka/K8S# kubectl expose deployment tomcat-deployment --type=LoadBalancer --port=8080 --target-port=8080 --name tomcat-load-balancer
 service "tomcat-load-balancer" exposed
 root@ubuntu:/home/luka/K8S#

``kubectl describe services tomcat-load-balancer``


Deploying to Kubernetes
^^^^^^^^^^^^^^^^^^^^^^^

``kubectl`` is your gateway to working with deployments.

List deployments:

``kubectl get deployments``

View status of deployment roll outs:

``kubectl rollout status``

Sample Output:

.. code-block:: bash

 root@ubuntu:/home/luka/K8S# kubectl rollout status deployment tomcat-deployment
 deployment "tomcat-deployment" successfully rolled out
 root@ubuntu:/home/luka/K8S#


Set the image of a deployment:

``kubectl set image``

Sample:

.. code-block:: bash

 root@ubuntu:/home/luka/K8S# kubectl set image deployment/tomcat-deployment tomcat=tomcat:9.0.1
 deployment.apps "tomcat-deployment" image updated
 root@ubuntu:/home/luka/K8S#

View the history of a rollout, including previous revisions:

``kubectl rollout history``

Sample :

.. code-block:: bash

 root@ubuntu:/home/luka/K8S# kubectl rollout history deployment/tomcat-deployment
 deployments "tomcat-deployment"
 REVISION  CHANGE-CAUSE
 1         <none>
 2         <none>
 root@ubuntu:/home/luka/K8S#

with --revision=2 we can see detailed inforamtion.

Labels
^^^^^^

You can label nearly anything in the Kubernetes world:

- Deployments
- Services
- Nodes

**nodeSelector** - is a properity on a deployment that uses labels and selectors to choose which nodes the master decides to run a given pod on

To accomplish our goal of running our deployment only on nodes with SSD we will:
	- Label a node as having an SSD
	- Define the nodeSelector on out deployment to match only nodes having the label we just defined

Sample Example:

.. code-block:: bash

 root@ubuntu:/home/luka/K8S# kubectl get nodes
 NAME       STATUS    ROLES     AGE       VERSION
 minikube   Ready     <none>    3h        v1.10.0
 root@ubuntu:/home/luka/K8S# kubectl label node minikube storageType=ssd
 node "minikube" labeled
 root@ubuntu:/home/luka/K8S#

Sample Output:

.. code-block:: bash

 root@ubuntu:/home/luka/K8S# kubectl describe node minikube
 Name:               minikube
 Roles:              <none>
 Labels:             beta.kubernetes.io/arch=amd64
                    beta.kubernetes.io/os=linux
                    kubernetes.io/hostname=minikube
                    storageType=ssd

					...


**nodeSelector** added in deployment.yaml file.


.. code-block:: bash

 apiVersion: apps/v1beta2
 kind: Deployment
 metadata:
   name: tomcat-deployment
 spec:
   selector:
     matchLabels:
       app: tomcat
   replicas: 1
   template:
     metadata:
       labels:
         app: tomcat
     spec:
       containers:
       - name: tomcat
         image: tomcat:9.0
         ports:
         - containerPort: 8080
       nodeSelector:
         storageType: ssd


Helath Checking
^^^^^^^^^^^^^^^

Two types of health checks:

- Readiness Probes: To determine when a Pod is "ready"
- Liveness Probes: To determine when a Pod is "healthy" or "unhealthy" after it has become ready.

Successful HTTP or TCP request to the Pod

Successful command execution on the Pod

**Making it real: a healthy Tomcat**


.. code-block:: bash

 apiVersion: apps/v1beta2
 kind: Deployment
 metadata:
   name: tomcat-deployment
 spec:
   selector:
     matchLabels:
       app: tomcat
   replicas: 4
   template:
     metadata:
       labels:
         app: tomcat
     spec:
       containers:
       - name: tomcat
         image: tomcat:9.0
         ports:
         - containerPort: 8080
         livenessProbe:
           httpGet:
             path: /
             port: 8080
           initialDelaySeconds: 30
           periodSeconds: 30
         readinessProbe:
           httpGet:
             path: /
             port: 8080
           initialDelaySeconds: 15
           periodSeconds: 3

**livenessProbe** and **readinessProbe** added

Sample Output:

.. code-block:: bash

 root@ubuntu:/home/luka/K8S# kubectl describe deployment/tomcat-deployment
 ...
    Liveness:     http-get http://:8080/ delay=30s timeout=1s period=30s #success=1 #failure=3
    Readiness:    http-get http://:8080/ delay=15s timeout=1s period=3s #success=1 #failure=3
 ...

Web Interface
^^^^^^^^^^^^^

Kubernetes WEB UI

Runs on your Kubernetes master(s).

Called the "Dashboard UI"

KUBECTL can create a proxy/tunnel for you in situations you do not :

``kubectl proxy``


Installing the dashboard:

``kubectl create -f https://raw.githubusercontent.com/kubernetes/dashboard/master/src/deploy/recommended/kubernetes-dashboard.yaml``

Accessing the dashboard:

``kubectl proxy``

**"http://localhost:8001/ui"**

Exercise
^^^^^^^^

Task:
- Deploy and scale MongoDB.

	- Success criteria: The current version of MongoDB is running on your Kubernetes cluseter with four replicas.

Hints:
- MongoDB listens on port 27017
- Offically supported MongoDB Docker images are available on Docker Hub

Use kubectl run to run the default mongo image:

.. code-block:: bash

 kubectl run mongo-exercise-1 --image=mongo --port=27017

Use kubectl scale to scale the deployment to 4 replicas:

.. code-block:: bash

 kubectl scale --replicas=4 deployment/mongo-exercise-1

Verfy:

.. code-block:: bash

 luka@ubuntu:~$ kubectl describe deployment/mongo-exercise-1
 Name:                   mongo-exercise-1
 Namespace:              default
 CreationTimestamp:      Mon, 07 May 2018 06:33:19 -0700
 Labels:                 run=mongo-exercise-1
 Annotations:            deployment.kubernetes.io/revision=1
 Selector:               run=mongo-exercise-1
 Replicas:               4 desired | 4 updated | 4 total | 4 available | 0 unavailable
 StrategyType:           RollingUpdate
 MinReadySeconds:        0
 RollingUpdateStrategy:  1 max unavailable, 1 max surge
 Pod Template:
   Labels:  run=mongo-exercise-1
   Containers:
    mongo-exercise-1:
     Image:        mongo
     Port:         27017/TCP
     Host Port:    0/TCP
     Environment:  <none>
     Mounts:       <none>
   Volumes:        <none>
 Conditions:
   Type           Status  Reason
   ----           ------  ------
   Available      True    MinimumReplicasAvailable
 OldReplicaSets:  <none>
 NewReplicaSet:   mongo-exercise-1-7fc9856465 (4/4 replicas created)
 Events:
   Type    Reason             Age   From                   Message
   ----    ------             ----  ----                   -------
   Normal  ScalingReplicaSet  5m    deployment-controller  Scaled up replica set mongo-exercise-1- 7fc9856465 to 1
   Normal  ScalingReplicaSet  2m    deployment-controller  Scaled up replica set mongo-exercise-1- 7fc9856465 to 4

Verify:

.. code-block:: bash

 luka@ubuntu:~$ kubectl get pods
 NAME                                READY     STATUS    RESTARTS   AGE
 mongo-exercise-1-7fc9856465-7g8gb   1/1       Running   0          3m
 mongo-exercise-1-7fc9856465-7rn4n   1/1       Running   0          3m
 mongo-exercise-1-7fc9856465-8xm8p   1/1       Running   0          7m
 mongo-exercise-1-7fc9856465-fxsvn   1/1       Running   0          3m
 luka@ubuntu:~$

Advanced Kubernetes Usage
-------------------------

DNS & Service Discovery
^^^^^^^^^^^^^^^^^^^^^^^

- DNS (Domain Name Service) translates names into IP addresses.
- Kubernetes has a built-in DNS service that is launched (and configured) automatically.
- Kubernetes configures kubelets to tell individual containers to use the DNS service's IP to resolve DNS names.
- Every Service in your Kubernetes cluster gets a DNS name.
- Kubernetes has a specific & consistent nomenclature for deciding what this DNS name is:
	- ``<my-service-name>.<my-namespace>.svc.cluster.local``

**A Practical Example**

- Setting up a WordPress and MySQL base.

``mysql-deployment.yaml``

.. code-block:: bash

 apiVersion: v1
 kind: Service
 metadata:
   name: wordpress-mysql
   labels:
     app: wordpress
 spec:
   ports:
     - port: 3306
   selector:
     app: wordpress
     tier: mysql
   clusterIP: None
 ---
 apiVersion: apps/v1beta2 # for versions before 1.8.0 use apps/v1beta1
 kind: Deployment
 metadata:
   name: wordpress-mysql
   labels:
     app: wordpress
 spec:
   selector:
     matchLabels:
       app: wordpress
       tier: mysql
   strategy:
     type: Recreate
   template:
     metadata:
       labels:
         app: wordpress
         tier: mysql
     spec:
       containers:
       - image: mysql:5.6
         name: mysql
         env:
         - name: MYSQL_ROOT_PASSWORD
           value: PASSWORDS_IN_PLAIN_TEXT_ARE_BAD_WE_WILL_SHOW_SOMETHING_MORE_SECURE_LATER
         ports:
         - containerPort: 3306
           name: mysql

``kubectl create -f mysql-deployment.yaml``


``wordpress-deployment.yaml``

.. code-block:: bash

 apiVersion: v1
 kind: Service
 metadata:
   name: wordpress
   labels:
     app: wordpress
 spec:
   ports:
     - port: 80
   selector:
     app: wordpress
     tier: frontend
   type: LoadBalancer
 ---
 apiVersion: apps/v1beta2 # for versions before 1.8.0 use apps/v1beta1
 kind: Deployment
 metadata:
   name: wordpress
   labels:
     app: wordpress
 spec:
   selector:
     matchLabels:
       app: wordpress
       tier: frontend
   strategy:
     type: Recreate
   template:
     metadata:
       labels:
         app: wordpress
         tier: frontend
     spec:
       containers:
       - image: wordpress:4.8-apache
         name: wordpress
         env:
         - name: WORDPRESS_DB_HOST
           value: wordpress-mysql
         - name: WORDPRESS_DB_PASSWORD
           value: PASSWORDS_IN_PLAIN_TEXT_ARE_BAD_WE_WILL_SHOW_SOMETHING_MORE_SECURE_LATER
         ports:
         - containerPort: 80
           name: wordpress


.. code-block:: bash

 luka@ubuntu:~/K8S$ kubectl create -f wordpress-deployment.yaml
 service "wordpress" created
 deployment.apps "wordpress" created

Get URL:

.. code-block:: bash

 luka@ubuntu:~/K8S$ minikube service wordpress --url
 http://192.168.99.100:32029
 luka@ubuntu:~/K8S$

Volumes
^^^^^^^

**Like disks, but with a bit more**

- Volumes can be considered just a directory, with some data, which containers in a pod can access.
- Kubernetes supports multiple types of volumes that take care of how that data is stored, presisted, and made available.
	- Support for a variety of cloud providers' block store products.
	- Support for SAN-type hardware, file systems, etc
	- Support for local volumes (for testing/minikube only! Not production)
- Certain types of volumes can also provide sharing of files between Pods by being maunted to multiple Pods simultaneously.


**Using Volumes**

- Pods can specify what volumes they need and where to mount them:

 - Using the spec.volumes field (what volumes they need)
 - Using the spec.containers.volumeMounts field (where to mount them)

- Processec in the container then see a filesystem view of the data in that Volume.

- Using Volumes lets us separate stateless portions of our application (the code) from stateful data:

 - The infrastructure can be scaled, maintained, and live separately from the data it works on/with.
 - Also may ease portability, backup, recovery, and other management tasks in well-architected systems.


**Using Persistent Volumes**

- Persistent Volumes are a Volumes designed to provide persistent disk-like functionality.
- Using them involves:
	- Provisioning a PresistentVolume
	- Estabilishing a PresistentVolumeClaim


**Creating a Volume**

 - PersistentVolumes are defined using a "PersistentVolume" definition that specifies their type, size, and how they can be accessed.

 - Their type and access type is highly dependent on the underlying media:
	- Local disk-like
	- Network mount
	- Cloud Block Storage Service
	- Directory on the Host (testing only not production).


.. code-block:: bash

 kind: PersistentVolume
 apiVersion: v1
 metadata:
  name: task-pv-volume
  labels:
   type: local
 spec:
  storageClassName: manual
  capacity:
   storage: 10Gi
  accessModes:
   - ReadWriteOnce
  hostPath:
   path: "/mnt/data"

**Claiming a Volume**

- Pods use PersistentVolumeClaims to request physical storage defined by PersistentVolumes.

- Kubernetes uses the Claim to look for a PersistentVolume that satisfies the claim's requirements:
	- If it finds a match , i binds the claim to the volume
	- If it cannot find a match, it results in an error.

.. code-block:: bash

 kind: PersistentVolumeClaim
 apiVersion: v1
 metadata:
  name: task-pv-claim
 spec:
  storageClassName: manual
  accessModes:
   - ReadWriteOnce
  resources:
   requests:
    storage: 3Gi


Example: Local volume and wordpress (and mysql)

``local-volumes.yaml``

.. code-block:: bash

 apiVersion: v1
 kind: PersistentVolume
 metadata:
   name: local-pv-1
   labels:
     type: local
 spec:
   capacity:
     storage: 20Gi
   accessModes:
     - ReadWriteOnce
   hostPath:
     path: /tmp/data/pv-1
 ---
 apiVersion: v1
 kind: PersistentVolume
 metadata:
   name: local-pv-2
   labels:
     type: local
 spec:
   capacity:
     storage: 20Gi
   accessModes:
     - ReadWriteOnce
   hostPath:
     path: /tmp/data/pv-2



``mysql-deployment.yaml``

.. code-block:: bash

 apiVersion: v1
 kind: Service
 metadata:
   name: wordpress-mysql
   labels:
     app: wordpress
 spec:
   ports:
     - port: 3306
   selector:
     app: wordpress
     tier: mysql
   clusterIP: None
 ---
 apiVersion: v1
 kind: PersistentVolumeClaim
 metadata:
   name: mysql-pv-claim
   labels:
     app: wordpress
 spec:
   accessModes:
     - ReadWriteOnce
   resources:
     requests:
       storage: 20Gi
 ---
 apiVersion: apps/v1beta2 # for versions before 1.8.0 use apps/v1beta1
 kind: Deployment
 metadata:
   name: wordpress-mysql
   labels:
     app: wordpress
 spec:
   selector:
     matchLabels:
       app: wordpress
       tier: mysql
   strategy:
     type: Recreate
   template:
     metadata:
       labels:
         app: wordpress
         tier: mysql
     spec:
       containers:
       - image: mysql:5.6
         name: mysql
         env:
         - name: MYSQL_ROOT_PASSWORD
           valueFrom:
             secretKeyRef:
               name: mysql-pass
               key: password
         ports:
         - containerPort: 3306
           name: mysql
         volumeMounts:
         - name: mysql-persistent-storage
           mountPath: /var/lib/mysql
       volumes:
       - name: mysql-persistent-storage
         persistentVolumeClaim:
           claimName: mysql-pv-claim


``wordpress-deployment.yaml``

.. code-block:: bash

 apiVersion: v1
 kind: Service
 metadata:
   name: wordpress
   labels:
     app: wordpress
 spec:
   ports:
     - port: 80
   selector:
     app: wordpress
     tier: frontend
   type: LoadBalancer
 ---
 apiVersion: v1
 kind: PersistentVolumeClaim
 metadata:
   name: wp-pv-claim
   labels:
     app: wordpress
 spec:
   accessModes:
     - ReadWriteOnce
   resources:
     requests:
        storage: 20Gi
 ---
 apiVersion: apps/v1beta2 # for versions before 1.8.0 use apps/v1beta1
 kind: Deployment
 metadata:
   name: wordpress
   labels:
     app: wordpress
 spec:
   selector:
     matchLabels:
       app: wordpress
       tier: frontend
   strategy:
     type: Recreate
   template:
     metadata:
       labels:
         app: wordpress
         tier: frontend
     spec:
       containers:
       - image: wordpress:4.8-apache
         name: wordpress
         env:
         - name: WORDPRESS_DB_HOST
           value: wordpress-mysql
         - name: WORDPRESS_DB_PASSWORD
           valueFrom:
             secretKeyRef:
               name: mysql-pass
               key: password
         ports:
         - containerPort: 80
           name: wordpress
         volumeMounts:
         - name: wordpress-persistent-storage
           mountPath: /var/www/html
       volumes:
       - name: wordpress-persistent-storage
         persistentVolumeClaim:
           claimName: wp-pv-claim

Secrets
^^^^^^^

- Secrets contain small amounts of data

- Secrets can be delivered to a pod in the form of:
	- A file placed on a volume at runtime containing the secret data (useful for certificates)
	- An environment variable referenced by the Pod and inserted at runtime into the environment by the kublet running the Pod - just like any other environment variable.

**How secrets are stored**

- Kubernetes provides separation for secrets, it does not provide strong encryption.

- Secrets are tipically Base64 encoded strings stored separately from configuration and injected at runtime.

- You can encode it manually or use Kubernetes' tools to do it for you.


** How secrets are structured**

- Secrets are key:value pairs, both the key and the value are arbitrary strings.

** Creating a Secret**

- From a file ( create a secret named generic db-user-pass with the username from the username.txt file and password from the password.txt file):

	- ``kubectl create secret generic db-user-pass --from-file=./username.txt --from-file=./password.txt``

- From the a literal on the command line:

	- ``kubectl create secret generic mysql-pass --from-literal=password=YOUR_PASSWORD``




**Using a Secret - as an environment variable**

mysql example:

.. code-block:: bash

 apiVersion: apps/v1beta2 # for versions before 1.8.0 use apps/v1beta1
 kind: Deployment
 metadata:
   name: wordpress-mysql
   labels:
     app: wordpress
 spec:
   selector:
     matchLabels:
       app: wordpress
       tier: mysql
   strategy:
     type: Recreate
   template:
     metadata:
       labels:
         app: wordpress
         tier: mysql
     spec:
       containers:
       - image: mysql:5.6
         name: mysql
         env:
         - name: MYSQL_ROOT_PASSWORD
           valueFrom:
             secretKeyRef:
               name: mysql-pass
               key: password
         ports:
         - containerPort: 3306
           name: mysql
         volumeMounts:
         - name: mysql-persistent-storage
           mountPath: /var/lib/mysql
       volumes:
       - name: mysql-persistent-storage
         persistentVolumeClaim:
            claimName: mysql-pv-claim


** Using a secret as a file**

.. code-block:: bash

 ...
 volumeMounts:
 - name: secret-storage
   mountPath: /etc/secretStore

   ...
 - name: secret-storage
   secret:
    secretName: mysql-pass

example for wordpress:

.. code-block:: bash

 apiVersion: v1
 kind: Service
 metadata:
   name: wordpress
  labels:
     app: wordpress
 spec:
   ports:
     - port: 80
   selector:
     app: wordpress
     tier: frontend
   type: LoadBalancer
 ---
 apiVersion: v1
 kind: PersistentVolumeClaim
 metadata:
   name: wp-pv-claim
   labels:
     app: wordpress
 spec:
   accessModes:
     - ReadWriteOnce
   resources:
     requests:
       storage: 20Gi
 ---
 apiVersion: apps/v1beta2 # for versions before 1.8.0 use apps/v1beta1
 kind: Deployment
 metadata:
   name: wordpress
   labels:
     app: wordpress
 spec:
   selector:
     matchLabels:
       app: wordpress
       tier: frontend
   strategy:
     type: Recreate
   template:
     metadata:
       labels:
         app: wordpress
         tier: frontend
     spec:
       containers:
       - image: wordpress:4.8-apache
         name: wordpress
         env:
         - name: WORDPRESS_DB_HOST
           value: wordpress-mysql
         - name: WORDPRESS_DB_PASSWORD
           valueFrom:
             secretKeyRef:
               name: mysql-pass
               key: password
         ports:
         - containerPort: 80
           name: wordpress
         volumeMounts:
         - name: wordpress-persistent-storage
           mountPath: /var/www/html
       volumes:
       - name: wordpress-persistent-storage
         persistentVolumeClaim:
           claimName: wp-pv-claim

To see secrets:

``kubectl get secret``

Usage and Resource Monitoring
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- We'll explore kubernetes' "native" stack with a few open source additions:
	- Heapster
	- InfluxDB
	- Grafana

**Heapster**

- Heapster is Kubernetes' Container cluseter Monitoring Solution

Enabling Heapster on minikube

- Heapster comes enabled on most cloud-based platforms

- On minikube, we'll have to enable the add on:
	- ``minikube addons enable heapster``

- Check influx and heapster pod status with:
	- ``kubectl get pods --namespace=kube-system``

output:

.. code-block:: bash

 luka@ubuntu:~/K8S$ kubectl get pods --namespace=kube-system
 NAME                          READY     STATUS             RESTARTS   AGE
 heapster-ktxcg                1/1       Running            0          1m
 influxdb-grafana-986qh        2/2       Running            0          1m
 kube-addon-manager-minikube   1/1       Running            0          20h
 kube-dns-86f6f55dd5-5nnkx     1/3       CrashLoopBackOff   96         20h
 kubernetes-dashboard-fbn9q    0/1       CrashLoopBackOff   51         20h
 luka@ubuntu:~/K8S$

- When pods are up access the Grafana Dashboard:
	- ''minikube addons open heapster``

Namespaces and Resource Quotas
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Namespaces create multiple virtual cluseters on the same physical cluseters, these virtual cluseters are called namespaces.
- Namespaces provide separation, when you start to need them, start using them.

**Resource limits in namespaces**

- Namespaces can be assigned ResourceQuota objects

- Each namespace should have at most one (but it is not requred)

- This will limit the amount of usage allowed by the objects in that namespace

- You can limit:
	- Compute
	- Storage
	- Memory
	- How many objects can exist

.. code-block:: bash

 apiVersion: v1
 kind: ResourceQuota
 metadata:
  name: compute-resources
 spec:
  hard:
   pods: "4"
   request.cpu: "1"
   request.memory: 1Gi
   limits.cpu: "2"
   limits.memory: 2Gi


**Usefull commands for namespaces**

- Create a namespace
	- ``kubectl create namespace <namespace name>``

- List namespaces
	- ``kubectl get namespaces``


**Practical example**

First step:

``kubectl create namespace cpu-limited-tomcat``

Assign limit

``kubectl create -f ./cpu-limits.yaml --namespace=cpu-limited-tomcat``

``cpu-limits.yaml``

.. code-block:: bash

 apiVersion: v1
 kind: ResourceQuota
 metadata:
   name: compute-resources
 spec:
   hard:
     limits.cpu: "400m"


.. note:: "400m" = 40% CPU

Deploy Tomcat

``kubectl apply -f ./tomcat-deployment.yaml --namespace=cpu-limited-tomcat``

``tomcat-deployment.yaml``

.. code-block:: bash

 apiVersion: apps/v1beta2
 kind: Deployment
 metadata:
   name: tomcat-deployment
 spec:
   selector:
     matchLabels:
       app: tomcat
   replicas: 3
   template:
     metadata:
       labels:
         app: tomcat
     spec:
       containers:
       - name: tomcat
         image: tomcat:9.0
         ports:
         - containerPort: 8080
         resources:
           requests:
             cpu: "200m"

See deployment status

``kubectl describe deployment tomcat-deployment --namespace=cpu-limited-tomcat``

Auto-Scaling
^^^^^^^^^^^^

- Horizontal Pod Autoscaler (HPA) is a Kubernetes facility that adjusts the number of replicas of a Pod to match observed average CPU utilization to a target specified by the user.

- There are a variety of configurable options - quite a few - the key takeaway is to know that HPA will create new Pods ( or remove Pods) from a replica to maintain average CPU utilization across all Pods to a level specified when you create your HPA - subject to conditions you specify.

**Creating an HPA**

For "our" wordpress-deployment:

``kubectl autoscale deployment wordpress --cpu-percent=50 --min=1 --max=10``

Practical example workflow:

``kubectl apply -f ./wordpress-deployment.yaml``

``kubectl autoscale deployment wordpress --cpu-percent=50 --min=1 —max=5``

In separate terminal window (next two commands):

``kubectl run -i --tty load-generator --image=busybox /bin/sh``

``while true; do wget -q -O- http://wordpress.default.svc.cluster.local; done``

``kubectl get hpa``


Auditing
^^^^^^^^

Course-Slides.pdf

Practical example:

1. Stop Minikube (if running)
2. Define an Audit Policy
3. Start Minikube

``audit-policy.yaml``

.. code-block:: bash

 # Log all requests at the Metadata level.
 apiVersion: audit.k8s.io/v1beta1
 kind: Policy
 rules:
 - level: Metadata

``minikubeStartWithAudit``

.. code-block:: bash

 #!/bin/sh
 minikube start  --extra-config=apiserver.Authorization.Mode=RBAC --extra-config=apiserver.Audit.LogOptions.Path=/var/logs/audit.log   --extra-config=apiserver.Audit.PolicyFile=/etc/kubernetes/addons/audit-policy.yaml

 ``minikube ssh``

Exercise
^^^^^^^^

Set up dependent of the "Ghost" open-source publishing platforms

``ghost-deployment.yaml``

.. code-block:: bash

 apiVersion: v1
 kind: Service
 metadata:
   name: ghost
    labels:
     app: ghost
 spec:
   ports:
     - port: 2368
   selector:
     app: ghost
     tier: frontend
   type: LoadBalancer
 ---
 apiVersion: v1
 kind: PersistentVolumeClaim
 metadata:
   name: ghost-pv-claim
   labels:
     app: ghost
 spec:
   accessModes:
     - ReadWriteOnce
   resources:
     requests:
       storage: 10Gi
 ---
 apiVersion: apps/v1beta2
 kind: Deployment
 metadata:
   name: ghost-deployment
 spec:
   selector:
     matchLabels:
       app: ghost
   replicas: 1
   template:
     metadata:
       labels:
         app: ghost
     spec:
       containers:
       - name: ghost
         image: ghost
         ports:
         - containerPort: 2368
         volumeMounts:
         - name: ghost-persistent-storage
           mountPath: /var/lib/ghost/content
       volumes:
       - name: ghost-persistent-storage
         persistentVolumeClaim:
           claimName: ghost-pv-claim

``local-volume.yaml``

.. code-block:: bash

 apiVersion: v1
 kind: PersistentVolume
 metadata:
   name: ghost-content-pv
   labels:
     type: local
 spec:
   capacity:
     storage: 10Gi
   accessModes:
     - ReadWriteOnce
   hostPath:
     path: /tmp/data/ghost-content-pv


SetUp Horizontal  Pod Autoscaler to scale when CPU hits 50%

``kubectl autoscale deployment ghost --cpu-percent=50 --min=1 --max=5``


Kubernetes in Production
------------------------

High Availability
^^^^^^^^^^^^^^^^^

Key kubernetes architecture items

- master
	- kube-apiserver
	- kube-controller-manager
	- kube-scheduler
- etcd



Setting up a reliable system:

1. Creating reliable nodes that, together, will form an highly available master implementation in following steps
2. Setting up a redundant, reliable data storage layer with clustered etcd
3. Starting replicated, load balanced API servers (kube-api)
4. Setting up master-elected kube-scheduler and kube-controller-manager daemons
5. Multiple Worker Nodes


Reliable nodes for our masters:

- Separate, independent Linux machines that will run master processes
- Should run kublet & monit


Reliable data storage layer:

- etcd is a system that stores key and value pairs that is the data in a Kubernetes cluster
- It should run on every node that will be a master
- Consult the etcd documentation on the variety of options on how etcd can provide even deeper levels of redundancy if needed

Replicated API servers:

- kube-api should run on all nodes that will be a master
- kube-api should be behind a network load balancer, this will vary with your environment you are running

Master elected components:

- Now that items are set up on reliable nodes, we have the pieces in place, but they aren’t actually running
- We need to ensure only one actor works on the data at a given time
- Each Scheduler and Controller Manager will be started with a **--leaderelect** flag that will use a lease-lock API between themselves to ensure only one instance of each is running at a given time

Masters
^^^^^^^


- Masters typically run three key processes:
	- kube-apiserver
	- kube-controller-manager
	- kube-scheduler
- These processes access and rely on etcd


SETTING UP A KUBERNETES CLUSTER ON AWS

- Amazon offers a managed Kubernetes service called Elastic Kubernetes Service
	- Managed master redundancy/High Availability
	- Highly automated setup
- We’ll use “kops”, a suite of software provided by Kubernetes maintainers to do a bit more manual work to set up our own cluster

Steps:

1. Download and install “kops”
2. Download, configure, and install the “aws” command line tool suite
3. Create an AWS S3 bucket as a “state store”
4. Create the cluster using kops


LET’S HAVE A LOOK AT OUR MASTER

- Find the master node
- Find deployments running on it
- Examine the deployments that comprise the master

STOPPING OUR CLUSTER

- Delete the cluster using kops
- To be completely safe, close the AWS account you created to ensure all resources are released and you are not charged (or wait until after you’ve completed this lecture series if you’d like to follow along in the following lectures)

Setting up high availability
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

KEY HA DESIGN CONSIDERATIONS

- How many masters (an odd number is critical)
- Where the masters are located (across different AWS availability zones)

KOPS CREATE CLUSTER

- The create cluster command we used in the last lecture allows us to specify High Availability options
- How many masters
- In which zones these masters are provisioned

LET’S CREATE A HIGH AVAILABILITY CLUSTER

1. kops create
2. Look at the nodes to see multiple masters
3. Deploy WordPress & MySQL whitout volumes

Volumes on AWS
^^^^^^^^^^^^^^

DYNAMIC PROVSIOINING

- When an existing Volume does not match a Claim, k8s maybe able to (based on the provider type and configuration) to provision Volumes to fulfill the Claim
- Depends on underlying implementation’s ability to create new Volumes (available space, permissions, etc)
	- Example: If underlying system is a cloud provider with enough quota and access levels allow, k8s may be able to create new Volumes
	- Example: If underlying system is a SAN filesystem and there is no more free space, k8s would not be able to create new Volumes (it can’t order and install more physical disks on its own, yet! ;) )


VOLUMES ON AWS

- Amazon Web Services (AWS) offers persistent disks via its Elastic Block Store (EBS)
- Kubernetes on AWS can use (and even dynamically provision) Volumes on EBS to match or meet PersistentVolumeClaims made by Pods


DEMO

Edit our existing existing MySQL & WordPress deployments to include
PersistentVolumeClaims exactly like we did in the Volumes lecture but
this time, we won’t create the PersistentVolumes, so this will require
Dynamic Provisioning

``wordpress-deployment.yaml``

.. code-block:: bash

 apiVersion: v1
 kind: Service
 metadata:
   name: wordpress
   labels:
     app: wordpress
 spec:
   ports:
     - port: 80
   selector:
     app: wordpress
     tier: frontend
   type: LoadBalancer
 ---
 apiVersion: v1
 kind: PersistentVolumeClaim
 metadata:
   name: wp-pv-claim
   labels:
     app: wordpress
 spec:
   accessModes:
     - ReadWriteOnce
   resources:
     requests:
       storage: 20Gi
 ---
 apiVersion: apps/v1beta2 # for versions before 1.8.0 use apps/v1beta1
 kind: Deployment
 metadata:
   name: wordpress
   labels:
     app: wordpress
 spec:
   selector:
     matchLabels:
       app: wordpress
       tier: frontend
   strategy:
     type: Recreate
   template:
     metadata:
       labels:
         app: wordpress
         tier: frontend
     spec:
       containers:
       - image: wordpress:4.8-apache
         name: wordpress
         env:
         - name: WORDPRESS_DB_HOST
           value: wordpress-mysql
         - name: WORDPRESS_DB_PASSWORD
           valueFrom:
             secretKeyRef:
               name: mysql-pass
               key: password
         ports:
         - containerPort: 80
           name: wordpress
         volumeMounts:
         - name: wordpress-persistent-storage
           mountPath: /var/www/html
       volumes:
       - name: wordpress-persistent-storage
         persistentVolumeClaim:
           claimName: wp-pv-claim

``mysql-deployment.yaml``

.. code-block:: bash

 apiVersion: v1
 kind: Service
 metadata:
   name: wordpress-mysql
   labels:
     app: wordpress
 spec:
   ports:
     - port: 3306
   selector:
     app: wordpress
     tier: mysql
   clusterIP: None
 ---
 apiVersion: v1
 kind: PersistentVolumeClaim
 metadata:
   name: mysql-pv-claim
   labels:
     app: wordpress
 spec:
   accessModes:
     - ReadWriteOnce
   resources:
     requests:
       storage: 20Gi
 ---
 apiVersion: apps/v1beta2 # for versions before 1.8.0 use apps/v1beta1
 kind: Deployment
 metadata:
   name: wordpress-mysql
   labels:
     app: wordpress
 spec:
   selector:
     matchLabels:
       app: wordpress
       tier: mysql
   strategy:
     type: Recreate
   template:
     metadata:
       labels:
         app: wordpress
         tier: mysql
     spec:
       containers:
       - image: mysql:5.6
         name: mysql
         env:
         - name: MYSQL_ROOT_PASSWORD
           valueFrom:
             secretKeyRef:
               name: mysql-pass
               key: password
         ports:
         - containerPort: 3306
           name: mysql
         volumeMounts:
         - name: mysql-persistent-storage
           mountPath: /var/lib/mysql
       volumes:
       - name: mysql-persistent-storage
         persistentVolumeClaim:
           claimName: mysql-pv-claim

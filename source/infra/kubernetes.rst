Kubernetes
==========

What is Kubernetes?
-------------------

- Came out of Google.
- Donated to CNCF in 2014 (open source).
- Written in Go/Golang
- https://github.com/kubernetes/kubernetes
- slack channel: slack.k8s.io
- Google projects Borg and Omega
- Version 1. shipped way back in July 2015.
- Greek word meaning helmsman (the person who steers a ship)
- K8s K(ubernete)s   8 characters replace


What and Why
^^^^^^^^^^^^

- Hey, Kubernetes, I've got this app, and consists of these containers, just run it for me please.
- Package app up as a container, give K8s a declarative manifest, and it'll take care of the rest...
- It's early days (but not baby early).
- Kubernetes is strongly positioned.
- Very platform agnostic (bare metal, VMs, cloud instances, open stack, pretty much anything).
- Kubernetes is moving FAST


Kubernetes Architecture
-----------------------

Big picture view
^^^^^^^^^^^^^^^^

- An Orchestrator for Microservice Apps.
- K8s cluster (One or more masters and a bunch of nodes).
	- Masters are in charge and the make the decision about which nodes to run work on.
	- Nodes run the actual work.
- Object called Deployment (YAML file), it's just a manifest that tells Kubernetes what our app should look like.
- "Give deployment to master and master tells nodes what to do" .


Masters
^^^^^^^

- "The Kubernetes Control Plane" .
- It runs on Linux (bare metal, VMs...).
- Don't run user workloads on "Master"


**kube-apiserver**

- Front-end to the control plane.
- Exposes the API (REST).
- Consumes JSON (via manifest files).

**cluster store - KV**

- Persistent storage.
- Cluster state and config.
- Uses etcd (open source distributed key value store, NoSQL database)
- Distributed, consistent, watchable...
- The "Source of truth" for the cluster.
- No etcd or cluster store, NO cluster.

**kube-controller-manager**

- Controller of controllers.
	- Node controller
	- Endpoints controller
	- Namespace controller
	- ...
- Watches for changes
- Helps maintain desired state.

**kube-scheduler**

- Whatches apiserver for new pods.
- Assign work to nodes.
	- affinity/anti-affinity
	- constraints
	- resources
 	- ...


**apiserver a.k.a master**

- The master is the brain of Kubernetes. Commands and queries come into the apiserver component od the master, we can stick authentication on at this point...
- Commands come in usually via the ``kubectl`` command line utility, but they are formatted as JSON.


Nodes
^^^^^

- Only the tree things we care about:
	- the kublet
	- the container runtime
	- the kube-proxy


**Kublet**

- The main Kubernetes agent on the node.
- Registers node with cluster.
- Watches apiserver on the master for work assignments.
- Instantiates pods.
- Reports back to master.
- Exposes endpoint on port :10255
- **/spec** ; **/healthz** , **/pods**
- Pod = one or more containers packaged together and deployed as a single unit.


**Container Engine**

- Does container management:
	- Pulling images
	- Starting/stopping containers
	- ..
- Pluggable:
	- Usually Docker
	- Can be rtk


**kube-proxy**

This is like the network brain od the node.

- Kubernetes networking:
	- Pod IP addresses
		- All containers in a pod share a single IP

	- Load balances accross all pods in service


Desired state and the Declarative model
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Kubernetes operates on a declarative model (YAML or JSON Describe desired state; Give apiserver the manifest files) .

**Example:**

- Desired state/ record of intent
	- 3 x nginx pods
- Actual state
	- 3 x nginx pods
- **GOOD**


- One of the nodes goes down. Desired state still says , three pods please! Than acutal state says: Only two.
- Than kubernetes fires up another pod on one of the surviving nodes.


Pods
^^^^

**Atomic units of scheduling**

- In VMWare it is VM
- In Docker world it is Container
- In Kubertnetes world is Pod

- Kubernetes run containers buy allways inside of pods.
- Pods can have multiple containers (advanced use-case).

**Pod is**

- Ring-fenced environment (Pod itself doesn't actually run anything. It's just a sandbox, od sorts, to run containers in.) .
	- Network stack
	- Kernel namespaces
	- ..
	- **n** containers (all containers in pod share the pod environment; two containers in pod share same IP)


**Pods and Scaling**

The unit of scaling in Kubernetes is also the pod (add or remove pods).

Multi-container Pods (ti make sense to put web server (main container) and log scraper (sidecar container) together in a pod).

- You can't have a single pod spread over multiple nodes.
- Pods are mortal.
	- Phase: pending
	- Phase: running
	- Phase: succeeded/failed

- If pod dies, new will come.

**Deploying Pods**

- Usually via higher level objects.


Services
^^^^^^^^

When pod dies they come back somewhere else in the cluster with totally different IPs.

- A service is just a Kubernetes object, like a deployment or a pod.
- We define it with a YAML manifest.
- Provides a stable IP and DNS name for pods (in this example, see video)
- A higher level stable abstraction points for multiple pods, and the provide load balancing.
- Lables (Prod, BE, 1.3  on Service and Back-and pods)



**lables**

- Are the simplest and the most powerful thing in Kubernetes


Deployments
^^^^^^^^^^^

- Replication controller

.. code-block:: bash

 apiVersion: v1
 kind: ReplicationController
 metadata:
  name:xyz
 spec:
  replicas: 4

- Deloyment

.. code-block:: bash

 apiVersion: extensions/v1beta1
 kind: Deployment
 matadata:
  name: xyz
 spec:
  replicas: 4


- Self documenting
- Versioned
- Spec-once deploy-many
- Simple rolling updates and rollbacks (DNA od deployments and we can run multiple concurrent versions; simple versioned rollbacks)
- REST objects
- Deployed via YAML or JSON manifest
- Deployed via apiserver
- Add features to Replication Controllers (Replica Sets)

Installing Kubernetes
---------------------

Module intro
^^^^^^^^^^^^

- SIMPLEST: Minikube > Google Container Engine (GKE) > AWS provider > Manuall install


Minikube
^^^^^^^^

- Best way of spinning up a local k8s environment.


Google Kontainer Engine
^^^^^^^^^^^^^^^^^^^^^^^

- It is Kubernetes!

- "Kubernetes as a service"

- GKE is layered on top of Google Compute Engine.


Installing in AWS with kops
^^^^^^^^^^^^^^^^^^^^^^^^^^^

- kops-kubernetes operations

**Pre-reqs** : kubect, kops, AWS CLI, IAM account (with: AmazonEC2fullaccess, AmazonRoute53FullAccess, IAMFullAccess, AmazonVPCFullAccess), Properly configured DNS (for the real world) - domain.

Manual install
^^^^^^^^^^^^^^

**kubeadm**

- Every node (master and nodes) needs:
	- Docker (or rkt)
	- Kubelet
	- Kubeadm
	- Kubectl
	- CNI (container network interface)

Working with pods
-----------------

Pod Theory
^^^^^^^^^^

- Smallest/atomic unit of scheduling
- Have one or more containers in a pod
- Scheduled on nodes (minions)
- One IP to one pod relationship (containers inside one pod use different port numbers)
- All containers in a pod share the same cgroup limits, they have got access to the same volumes, the same network, an even the sam IPC namespaces.
- Inside pod containers talk over the shared localhost interface


**Pods lifecycle**

- manifest file (YAML or JSON) >>> apiserver >>> pending >>> running >>> succeeded

- When they die, they die, there is no bringing them back from the dead.

Deploying your first pod
^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

 apiVersion: v1
 kind: Pod
 metadata:
   name: hello-pod
   labels:
     zone: prod
     version: v1
 spec:
   containers:
   - name: hello-ctr
     image: nigelpoulton/pluralsight-docker-ci:latest
     ports:
     - containerPort: 8080



``kubectl create -f pod.yml``


Deploying pods via Replication Controllers
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Implements desired state


Example:

.. code-block:: bash

 apiVersion: v1
 kind: ReplicationController
 metadata:
    name: hello-rc
 spec:
    replicas: 5
 ....


- Reconciliation loop



.. code-block:: bash

 apiVersion: v1
 kind: ReplicationController
 metadata:
  name: hello-rc
 spec:
  replicas: 10
  selector:
   app: hello-world
  template:
   metadata:
    labels:
     app: hello-world
   spec:
    containers:
    - name: hello-pod
      image: nigelpoulton/pluralsight-docker-ci:latest
      ports:
      - containerPort: 8080

``kubectl create -f rc.yml``

After change of yml file:

``kubectl apply -f rc.yml``


Kubernetes Services
-------------------

The theory
^^^^^^^^^^

**Services**

- REST objects in th k8s API
- Apstraction


- Service (IP=reliable; DNS=reliable; Port=Reliable) NEVER CHANGES
- Endpoint (Pod1 IP, Pod2 IP, Pod3 IP,...)
- Label selector (zone=prod, varsion=v1)


**Service discovery**

- DNS based (best)
    - Services get DNS names that are resolvable inside the cluster.
- Environment variables


Creating a Service the Iterative Way
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


``kubectl expose rc hello-rc --name=hello-svc --target-port=8080 --type=NodePort``

Crating a Service the Declarative way
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``scv.yml``

.. code-block:: bash

 apiVersion: v1
 kind: Service
 metadata:
  name: hello-svc
  labels:
   api: hello-world
 spec:
  type: NodePort
  ports:
  - port: 8080
    nodeport: 30001
    protocol: TCP
  selector:
    app: hello-world

**ServiceType**
- ClusterIP: Stable internal cluster IP
- NodePort: Exposes the app outside of the cluster by adding a cluster-wide port on top of ClusterIP
- LoadBalancer: Integrates NodePort with cloud-based load balancers.

``kubectl create -f svc.yml``

In the real world
^^^^^^^^^^^^^^^^^

**Updating Bussiness Apps**

- Power of labels.
- ver 17.06.02 >>> 17.06.03


Kubernetes Deployments
----------------------

Deployment theory
^^^^^^^^^^^^^^^^^

- **Rolling updates and simple rollbacks**
- Replica set (Replication Controller v2 )

``kubectl rolling-update -f updated-rc.yml``

YAML file >>> Master (apiserver) >>> Deployed to cluster

Creating your first Kubernetes Deployment
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


.. code-block:: bash

 apiVersion: extensions/v1beta1
 kind: Deployment
 metadata:
  name: hello-deploy
 spec:
  replicas: 10
  template:
   metadata:
     labels:
      app: hello-world
   spec:
    containers:
    - name: hello-pod
      image: nigelpoulton/pluralsight-docker-ci:latest
      ports:
      - containerPort: 8080


Updating a Deployment
^^^^^^^^^^^^^^^^^^^^^


.. code-block:: bash

 apiVersion: extensions/v1beta1
 kind: Deployment
 metadata:
  name: hello-deploy
 spec:
  replicas: 10
  minReadySeconds: 10
  strategy:
   type: RollingUpdate
   rollingUpdate:
    maxUnavailable: 1
    masSurge: 1
  template:
   metadata:
     labels:
      app: hello-world
   spec:
    containers:
    - name: hello-pod
      image: nigelpoulton/pluralsight-docker-ci:edge
      ports:
      - containerPort: 8080


``kubectl apply -f deploy.yml --record``

``kubectl rollout history deployment hello-deploy``

``kubectl rollout undo deployment hello-deploy --to-revision=1``

``kubectl rollout status deployment hello-deploy``

























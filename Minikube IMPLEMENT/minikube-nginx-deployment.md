# Minikube Nginx Deployment Report

## 1. Experiment Goal

Deploy the first Kubernetes application locally with Minikube, expose an Nginx Deployment through a NodePort Service, and verify access from a browser.

## 2. Environment

- OS: Windows
- Container runtime: Docker Desktop
- Kubernetes environment: Minikube
- Kubernetes CLI: kubectl
- Application image: `nginx`

## 3. K3s/minikube Installation Proof

This experiment uses Minikube instead of K3s.

Recommended screenshot:

![Minikube installation proof](./images/minikube-install-proof.png)

Cluster node proof:

![kubectl get nodes output](./images/kubectl-get-nodes.png)

Verified tool and cluster information:

```cmd
docker info
```

Key output:

```text
Server Version: 29.4.3
Operating System: Docker Desktop
OSType: linux
Architecture: x86_64
Name: docker-desktop
```

```cmd
tools\kubectl.exe get nodes
```

Output:

```text
NAME       STATUS   ROLES           AGE    VERSION
minikube   Ready    control-plane   115s   v1.31.0
```

## 4. Nginx Deployment Steps

Create the Nginx Deployment:

```cmd
tools\kubectl.exe create deployment nginx --image=nginx
```

Output:

```text
deployment.apps/nginx created
```

Expose the Deployment as a NodePort Service:

```cmd
tools\kubectl.exe expose deployment nginx --port=80 --type=NodePort
```

Output:

```text
service/nginx exposed
```

## 5. kubectl get pods Output

Recommended screenshot:

![kubectl get pods output](./images/kubectl-get-pods.png)

Command:

```cmd
tools\kubectl.exe get pods,deploy,svc
```

Output:

```text
NAME                         READY   STATUS    RESTARTS   AGE
pod/nginx-676b6c5bbc-vl7hv   1/1     Running   0          5m49s

NAME                    READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx   1/1     1            1           5m49s

NAME                 TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
service/kubernetes   ClusterIP   10.96.0.1       <none>        443/TCP        7m50s
service/nginx        NodePort    10.100.97.165   <none>        80:32287/TCP   5m42s
```

## 6. Nginx Access Verification

Get the service URL:

```cmd
tools\minikube-v1.34.0.exe service nginx --url
```

Output:

```text
http://127.0.0.1:54390
```

Open the URL in a browser:

```text
http://127.0.0.1:54390
```

Recommended screenshot:

![Nginx access success](./images/nginx-access-success.png)

The browser displayed the default Nginx welcome page:

```text
Welcome to nginx!
```

## 7. Result

The experiment was completed successfully. The Nginx Pod is running, the Deployment is available, the Service is exposed through NodePort, and the Nginx welcome page is accessible from the browser.

## 8. Kubernetes Concepts

- Pod: the smallest deployable unit in Kubernetes, used here to run the Nginx container.
- Deployment: manages the desired state of the Nginx Pod and keeps one replica available.
- Service: provides a stable access endpoint for Pods. The `NodePort` type exposes the Nginx application for local access through Minikube.

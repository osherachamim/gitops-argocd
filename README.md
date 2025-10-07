  

* * *

🌍 GitOps with Argo CD
======================

Overview
--------

This repository demonstrates **GitOps workflows** using **Argo CD** to deploy and manage multiple Kubernetes applications.

Each application folder contains its own Kubernetes manifests (`Deployment`, `Service`, etc.), and Argo CD automatically syncs the desired state from this repository to the Kubernetes cluster.

* * *

📁 Repository Structure
-----------------------

    gitops-argocd/
    ├── hello-word-go-app/     # Go "Hello World" application (Dockerized)
    ├── simple-nginx-app/      # Simple NGINX deployment for testing
    ├── solar-system/          # Example app for namespace & monitoring testing
    └── LICENSE
    

Each directory represents an independent application managed by **Argo CD**.

* * *

🚀 What is Argo CD?
-------------------

[Argo CD](https://argo-cd.readthedocs.io/en/stable/) is a declarative, GitOps continuous delivery tool for Kubernetes.

It continuously monitors Git repositories for changes and automatically synchronizes the live cluster state to match the declared manifests in Git.

* * *

⚙️ How to Run These Apps in Argo CD
-----------------------------------

### 1\. Connect the Repository

In Argo CD UI:

*   Go to **Settings → Repositories → Connect Repo**
    
*   Add this GitHub repo URL:
    
        https://github.com/osherachamim/gitops-argocd
        
    

You should see the connection status as ✅ **Successful**.

* * *

### 2\. Create a New Application

Click **NEW APP** and configure:

Field

Example Value

Application Name

`hello-world-go`

Project

`default`

Repository URL

`https://github.com/osherachamim/gitops-argocd`

Revision

`HEAD`

Path

`./hello-word-go-app`

Destination Server

`https://kubernetes.default.svc`

Namespace

`go-applications`

Then click **Create** → **Sync**.

* * *

### 3\. Verify Deployment

Run in terminal:

    kubectl -n go-applications get pods,svc
    

Expected:

    NAME                                READY   STATUS    AGE
    pod/hello-world-go-xxxxxxx-yyyyy     1/1     Running   m
    
    NAME                        TYPE       CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
    service/hello-world-go-svc  NodePort   10.x.x.x                 80:30080/TCP   m
    

Access your app:

    http://:
    

* * *

🔄 Update Flow (GitOps Style)
-----------------------------

1.  Update your code / image version.
    
2.  Push changes to Git (`git commit` → `git push`).
    
3.  Argo CD detects the change → marks app as **OutOfSync**.
    
4.  Click **SYNC** to apply changes to the cluster.
    
    *   Kubernetes creates a new **ReplicaSet** → new Pod(s).
        
    *   Old ReplicaSet remains for rollback if needed.
        

* * *

🧠 Example Applications
-----------------------

App

Description

Path

**hello-word-go-app**

Simple Go web app (Docker + Deployment + Service)

`./hello-word-go-app/`

**simple-nginx-app**

Minimal NGINX deployment example

`./simple-nginx-app/`

**solar-system**

Namespace + Service definition example

`./solar-system/`

* * *

🧩 Rollback Example
-------------------

If you want to revert to a previous version:

    kubectl -n go-applications rollout undo deployment hello-world-go
    

* * *

📜 License
----------

This repository is licensed under the MIT License.

* * *


DockerCoins Application with CI/CD based on ArgoCD tool.


App architecture

![Dockercoins app architecture](./dockercoins.png)


CI/CD part:

In order to implement the application you must first install the ArgoCD application into the Kubernetes cluster. You can do so by following those steps:

1. Connect locally to the Kubernetes Cluster:
    a. Setup gcloud authentication plugin:
    ```
    gcloud components install gke-gcloud-auth-plugin
    ```

    b. Connect to the Kubernetes cluster by importing the config file:
    ```
    gcloud container clusters get-credentials dockercoins-cluster-teo --zone europe-west1-b --project teolia-school-devops
    ```

2. Install ArgoCD:
    a. Create ArgoCD namespace and install components:
    ```
    kubectl create namespace argocd
    kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/core-install.yaml
    ```

    b. Apply Argo CD configuration(`manifests/argocd-app.yml`)
    ```
    kubectl apply -f manifests/argocd-app.yml
    ```
    This will point ArgoCD to the `https://github.com/AmineBouzayeni/prod-manifests.git` repository which contains the production manifests.

    c. You can use Kubectl Port Forwarding to connect locally to ArgoCD locally via `localhost:8080` for example:
    ```
    kubectl port-forward svc/argocd-server -n argocd 8080:443
    ```

3. Login to ArgoCD:
    The username is `admin`. The password can be found using the `get secret` command of kubectl:
    ```
    kubectl get secret argocd-initial-admin-secret -n argocd -o yaml
    ```
    Take the password field value and decode it using a base64 decoder.


Pipeline architecture:

![Pipeline architecture](./TeoSchool_argo.png)


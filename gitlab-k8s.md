### ✅ 1. Create GitLab Project

1. **Login to GitLab**: Go to [https://gitlab.com](https://gitlab.com) and log in to your GitLab account.
2. **Create New Project**:

   * Click the **“New Project”** button.
   * Choose **"Create blank project"**.
   * Fill in:

     * **Project name** (e.g., `k8s-agent-project`)
     * Set **visibility level** (Public/Private).
   * Click **“Create project”**.

---

### ✅ 2. Navigate to Kubernetes Cluster Integration

1. Go to your **newly created GitLab project**.
2. In the left sidebar, click on **“Deployments” > “Kubernetes clusters”**.
3. Click on the **“Connect a cluster (agent)”** button.

---

### ✅ 3. Connect to Cluster

1. In the "Connect a cluster" screen, give the **Agent Name** (e.g., `my-agent`) and click **“Register”**.
2. GitLab will create a **`config.yaml`** for the agent and show you the **Helm commands** to install the agent in your Kubernetes cluster.

---

### ✅ 4. Run Helm Commands to Install and Configure the Agent

> **Make sure your kubeconfig is configured and pointing to the correct cluster**

Open your terminal and run the commands provided by GitLab. They will be like:

```bash
# Add GitLab Helm repository
helm repo add gitlab https://charts.gitlab.io

# Update the Helm repo
helm repo update

# Create the namespace for GitLab agent
kubectl create namespace gitlab-agent

# Install the agent using Helm
helm upgrade --install gitlab-agent gitlab/gitlab-agent \
  --namespace gitlab-agent \
  --set config.token=<your-token> \
  --set config.kasAddress=wss://kas.gitlab.com
```

> Replace `<your-token>` with the actual token provided on the GitLab UI.

---

### ✅ 5. Check if the Agent is Connected

1. Go back to GitLab.
2. In the **“Kubernetes clusters”** page under your project, check the **Agent Status**:

   * If connected properly, it will show as **“Connected”**.

3. create config file in your repo

Location: .gitlab/agents/[agent-name]/config.yaml

```
ci_access:
  # Allow GitLab CI jobs to access the Kubernetes API through the agent
  projects:
    - id: <GroupNamespace>/<ProjectName>
```

4. Create gitlabcicd file to test the kubernetes connection

```
variables:
  KUBE_CONTEXT: <GroupNamespace>/<ProjectName>:<AgentName>

stages:
  - info

get-cluster-info:
  stage: info
  image:
    name: bitnami/kubectl:latest
    entrypoint: ['']
  script:
    - kubectl config use-context $KUBE_CONTEXT
    - kubectl get nodes -o wide
    - kubectl get pods --all-namespaces
``



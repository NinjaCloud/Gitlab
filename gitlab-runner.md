## 🧪 **Lab: Install GitLab Runner using Binary File**

### 🖥️ **Objective**:

Install GitLab Runner from the official binary, create a system user, run it as a service, and register it with your GitLab project.

---

## 🔧 Prerequisites

* Ubuntu VM (any recent version)
* Docker installed (`docker --version`)
* GitLab project with access to **Runner Token**
* Root/sudo access on VM

---

## 🪜 **Steps**

### ✅ Step 1: Download GitLab Runner Binary

```bash
sudo curl -L --output /usr/local/bin/gitlab-runner \
"https://s3.dualstack.us-east-1.amazonaws.com/gitlab-runner-downloads/latest/binaries/gitlab-runner-linux-amd64"
```

> 📦 This downloads the latest GitLab Runner binary to `/usr/local/bin`.

---

### ✅ Step 2: Make It Executable

```bash
sudo chmod +x /usr/local/bin/gitlab-runner
```

---

### ✅ Step 3: Create GitLab Runner User

```bash
sudo useradd --comment 'GitLab Runner' --create-home gitlab-runner --shell /bin/bash
```

> 👤 Creates a dedicated user `gitlab-runner` to run the service securely.

---

### ✅ Step 4: Install GitLab Runner as a Service

```bash
sudo gitlab-runner install --user=gitlab-runner --working-directory=/home/gitlab-runner
```

### ✅ Step 5: Start the GitLab Runner Service

```bash
sudo gitlab-runner start
```

---

### ✅ Step 6: Register the GitLab Runner

```bash
sudo gitlab-runner register
```

#### During the prompt:

1. **GitLab instance URL**: `https://gitlab.com` (or your self-hosted URL)
2. **Registration token**: Get from `Project > Settings > CI/CD > Runners > Registration Token`
3. **Description**: `my-docker-runner`
4. **Tags**: `docker`
5. **Executor**: `docker`
6. **Docker image**: `alpine:latest` (or your preferred base image)

---

## ✅ Step 7: Verify Runner Status

Check registered runner:

```bash
sudo gitlab-runner list
```

Check GitLab UI:
Go to `Project > Settings > CI/CD > Runners`

* You should see your runner listed as **online** with green dot.

---

### 🧼 Optional: Restart GitLab Runner (if needed)

```bash
sudo gitlab-runner restart
```


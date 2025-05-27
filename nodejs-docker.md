## 🔧 Prerequisites

* GitLab account
* GitLab-managed runner enabled (default for most new GitLab users)
* GitLab project/repository created

---

## 📁 1. Project Structure

Create a project with the following structure:

```
nodejs-docker-ci/
│
├── .gitlab-ci.yml
├── Dockerfile
├── package.json
├── app.js
```

---

## 🧾 2. `package.json`

```json
{
  "name": "nodejs-gitlab-ci",
  "version": "1.0.0",
  "description": "A simple Node.js app to demonstrate GitLab CI with Docker",
  "main": "app.js",
  "scripts": {
    "start": "node app.js"
  },
  "dependencies": {
    "express": "^4.18.2"
  }
}
```

---

## 🧠 3. `app.js`

```js
const express = require('express');
const app = express();
const port = process.env.PORT || 3000;

app.get('/', (req, res) => res.send('Hello from GitLab CI/CD pipeline!'));

app.listen(port, () => {
  console.log(`App running on port ${port}`);
});
```

---

## 🐳 4. `Dockerfile`

```Dockerfile
# Base image
FROM node:18-alpine

# Create app directory
WORKDIR /usr/src/app

# Install dependencies
COPY package*.json ./
RUN npm install

# Bundle app source
COPY . .

# App runs on port 3000
EXPOSE 3000

CMD ["npm", "start"]
```

---

## 🔐 5. GitLab Container Registry Setup

1. Go to your GitLab project.
2. Navigate to `Settings` → `Visibility, project features, permissions`.
3. Ensure **Container Registry** is enabled.

---

## 🔐 6. Add CI/CD Variables

Go to `Settings` → `CI/CD` → `Variables` and add:

| Key                    | Value                          | Type   |
| ---------------------- | ------------------------------ | ------ |
| `CI_REGISTRY`          | `registry.gitlab.com`          | Plain  |
| `CI_REGISTRY_USER`     | `<your-gitlab-username>`       | Plain  |
| `CI_REGISTRY_PASSWORD` | `<your-personal-access-token>` | Masked |

📌 Generate a [Personal Access Token](https://gitlab.com/-/profile/personal_access_tokens) with `read_registry` and `write_registry` scopes.

---

## 🧾 7. `.gitlab-ci.yml`

```yaml
image: docker:latest

services:
  - docker:dind

stages:
  - build
  - deploy

variables:
  IMAGE_NAME: registry.gitlab.com/$CI_PROJECT_NAMESPACE/$CI_PROJECT_NAME

before_script:
  - docker login -u "$CI_REGISTRY_USER" -p "$CI_REGISTRY_PASSWORD" $CI_REGISTRY

build:
  stage: build
  script:
    - docker build -t $IMAGE_NAME:$CI_COMMIT_SHORT_SHA .
    - docker push $IMAGE_NAME:$CI_COMMIT_SHORT_SHA

deploy:
  stage: deploy
  script:
    - echo "Image pushed:$IMAGE_NAME:$CI_COMMIT_SHORT_SHA"
```




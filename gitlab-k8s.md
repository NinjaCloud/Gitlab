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
```

### Config File 

Location: .gitlab/agents/<agent-name>/config.yaml

```
ci_access:
  # Allow GitLab CI jobs to access the Kubernetes API through the agent
  projects:
    - id: <GroupNamespace>/<ProjectName>
```

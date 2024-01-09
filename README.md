# Create ssh

Modify `argocd-cm` ConfigMap:

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: argocd-cm
data:
  resource.exclusions: |
    - apiGroups:
      - "*"
      kinds:
      - "PipelineRun"
      - "TaskRun"
      clusters:
      - "*"
```

```
apiVersion: v1
kind: Secret
metadata:
  name: gh-apps-ssh-key
  namespace: vadym
  annotations:
    tekton.dev/git-0: github.com # Described below
type: kubernetes.io/ssh-auth
stringData:
  ssh-privatekey: -----BEGIN OPENSSH PRIVATE KEY-----

  # This is non-standard, but its use is encouraged to make this more secure.
  # If it is not provided then the git server's public key will be requested
  # when the repo is first fetched.
  known_hosts: github.com

```

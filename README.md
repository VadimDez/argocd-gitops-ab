# Create ssh

Modify `argocd` instance, add to `spec``:

```
resourceExclusions: |
  - apiGroups:
    - tekton.dev
    clusters:
    - '*'
    kinds:
    - TaskRun
    - PipelineRun
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

Add line to the `ServiceAccounts -> pipeline` to secrets and imagePullSecrets:

```
  - name: ibm-entitlement-key
```

### Password for ACE UI

Porject = `ibm-common-services`

Secrets -> integration-admin-initial-temporary-credentials

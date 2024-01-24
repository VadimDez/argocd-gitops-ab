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

### Github secret:

Use this to generate ssh keys:

```
ssh-keygen -t ed25519 -C "vadym.yatsyuk@gmail.com"
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

```
apiVersion: v1
kind: Secret
metadata:
  name: gh-config-repo-ssh-key
  namespace: vadym
stringData:
  id_ed25519: |
    -----BEGIN OPENSSH PRIVATE KEY-----
    a
    b
    c
    d
    e
    -----END OPENSSH PRIVATE KEY-----
  known_hosts: |
    github.com ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIOMqqnkVzrm0SdG6UOoqKLsabgH5C9okWi0dh2l9GKJl
    github.com ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBEmKSENjQEezOmxkZMy7opKgwFB9nkt5YRrYMjNuG5N87uRgg6CLrbo5wAdT/y6v0mKV0U2w0WZ2YB/++Tpockg=
    github.com ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCj7ndNxQowgcQnjshcLrqPEiiphnt+VTTvDP6mHBL9j1aNUkY4Ue1gvwnGLVlOhGeYrnZaMgRK6+PKCUXaDbC7qtbW8gIkhL7aGCsOr/C56SJMy/BCZfxd1nWzAOxSDPgVsmerOBYfNqltV9/hWCqBywINIR+5dIg6JTJ72pcEpEjcYgXkE2YEFXV1JHnsKgbLWNlhScqb2UmyRkQyytRLtL+38TGxkxCflmO+5Z8CSSNY7GidjMIZ7Q4zMjA2n1nGrlTDkzwDCsw+wqFPGQA179cnfGWOWRVruj16z6XyvxvjJwbz0wQZ75XK5tKSb7FNyeIEs4TT4jk+S4dhPeAUC5y+bDYirYgM4GC7uEnztnZyaVWQ7B381AK4Qdrwt51ZqExKbQpTUNn+EjqoTwvqNj4kqx5QUCI0ThS/YkOxJCXmPUWZbhjpCg56i+2aB6CmK2JGhn57K5mj0MNdBXA4/WnwH6XoPWJzK5Nyu2zB3nAZp+S5hpQs+p1vN1/wsjk=
  config: |
    Host github.com
    User git
    Port 22
    Hostname github.com
    IdentityFile ~/.ssh/id_ed25519
    TCPKeepAlive yes
    IdentitiesOnly yes
```

### Image pull secrets

Add line to the `ServiceAccounts -> pipeline` to secrets and imagePullSecrets:

```
  - name: ibm-entitlement-key
```

### Password for ACE UI

Porject = `ibm-common-services`

Secrets -> integration-admin-initial-temporary-credentials

### Get Nexus password

```
oc exec <pod> -n vadym -- cat /nexus-data/admin.password
```

cyhxer-1pykPy-dypham

```
oc exec vadym-nexusrepo-sonatype-nexus-7f7c79dfc-8t5jc -n vadym -- cat /nexus-data/admin.password
```

### Git auth

https://tekton.dev/docs/pipelines/auth/#configuring-ssh-auth-authentication-for-git

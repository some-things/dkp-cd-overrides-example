# dkp-cd-overrides-example

## How-to

```
kubectl create namespace dkp-cd-overrides

cat <<EOF | kubectl apply -f -
apiVersion: source.toolkit.fluxcd.io/v1beta2
kind: GitRepository
metadata:
  name: dkp-cd-overrides
  namespace: dkp-cd-overrides
spec:
  gitImplementation: go-git
  interval: 1m0s
  ref:
    branch: main
  timeout: 1m0s
  url: https://github.com/some-things/dkp-cd-overrides-example.git
EOF

cat <<EOF | kubectl apply -f -
apiVersion: kustomize.toolkit.fluxcd.io/v1beta2
kind: Kustomization
metadata:
  name: dkp-cd-overrides
  namespace: dkp-cd-overrides
spec:
  interval: 10m0s
  path: "./workspaces/kommander/application-overrides"
  prune: true
  sourceRef:
    kind: GitRepository
    name: dkp-cd-overrides
    namespace: dkp-cd-overrides
EOF
```
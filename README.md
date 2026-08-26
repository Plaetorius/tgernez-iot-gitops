# tgernez-iot-gitops

Deployment source of truth for **Inception-of-Things — Part 3**.

An Argo CD `Application` running in a local K3d cluster watches this repository
and continuously reconciles the `dev` namespace to match `manifests/`.

```
manifests/
├── deployment.yaml   # wil42/playground - the image tag drives the release
├── service.yaml
└── ingress.yaml
```

## Rolling the application forward

Nothing is applied by hand. Change the tag here, commit, push:

```bash
sed -i 's|wil42/playground:v1|wil42/playground:v2|' manifests/deployment.yaml
git commit -am "release: v2"
git push
```

Argo CD picks up the commit and updates the cluster:

```bash
curl http://localhost:8888/
```

Rolling back is the same operation in reverse — `v2` back to `v1`.

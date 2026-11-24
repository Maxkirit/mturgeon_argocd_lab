This is the repo for a school project. We're using Argo CD on a k3d cluster to deploy app. 
This repo is the target application repo. Change the docker image version in `deployment.yaml` and Argo CD will deploy it.

For reference, a much more standard application repo structure leveraging Kustomize layering would look like this:

```
myapp-config/
├── README.md
└── apps/
    └── myapp/
        ├── base/
        │   ├── kustomization.yaml
        │   ├── deployment.yaml
        │   ├── service.yaml
        │   └── ingress.yaml
        └── overlays/
            ├── dev/
            │   └── kustomization.yaml
            └── prod/
                └── kustomization.yaml
```

with the following ideas behind each file:
- `base/kustomization.yaml`: holds the resoucres "map" for the app (tells which files compose the layers)
- `deployment.yaml`: target replicas, port forwarding, readiness probes, nodes config, etc
- `service.yaml`: service discovery info for the cluster server
- `ingress.yaml`: networking info for the loadbalancer (Traefik by default on k3d)
- `overlays/<env>/kustomization.yaml`: the added configs for each of the target environments for your app. In there would go the actual images and tags as well as cluster configs overwrites
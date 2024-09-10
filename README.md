# [Backstage](https://backstage.io)

This is your newly scaffolded Backstage App, Good Luck!

To start the app, run:

```sh
yarn install
yarn dev
```

Useful commands for Kubernetes

```sh
yarn build:all
docker image build . -f packages/backend/Dockerfile --tag backstage
helm install cbs-backstage ./charts/backstage/
kubectl logs deployment/cbs-backstage
kubectl port-forward svc/cbs-backstage 7007:7007


helm uninstall cbs-backstage
```

Useful commands for Docker

```sh
docker run -it -p 7007:7007 backstage
```


## Tips

To get the Helm Charts working with a local image you need to
- Change the `values.yaml` line `pullPolicy: Always` to `pullPolicy: IfNotPresent`
- Comment out `registry: ghcr.io` so that it uses your local registry
- Change `repository: backstage\backstage` to be the name of your built image tag, so `repository: backstage` for me
- Set `guest: null` in the `app-config.yaml` if it's not being overridden by another config... or
- Args can be used like `args: ["--config", "app-config.yaml", "--config", "app-config.production.yaml"]`
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

## Set up Argo

https://argo-cd.readthedocs.io/en/stable/getting_started/

```sh
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
kubectl port-forward svc/argocd-server -n argocd 8080:443

argocd admin initial-password -n argocd
argocd login localhost:8080

kubectl config get-contexts -o name
argocd cluster add docker-desktop

kubectl config set-context --current --namespace=argocd
argocd app create guestbook --repo https://github.com/argoproj/argocd-example-apps.git --path guestbook --dest-server https://kubernetes.default.svc --dest-namespace default
argocd app get guestbook
argocd app sync guestbook

```

```sh   
argocd app create backstage --repo https://github.com/AnthonyWard/backstage-argo.git --path charts/backstage --dest-server https://kubernetes.default.svc --dest-namespace default
kubectl port-forward svc/backstage 7007:7007 -n default
```
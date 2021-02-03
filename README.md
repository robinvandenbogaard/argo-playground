# argo-playground
Repo to experiment with on a local docker for windows k8s cluster with ArgoCD

# Setup
Followed this guide: https://argoproj.github.io/argo-cd/getting_started/
Steps taken on a fresh docker for windows k8s cluster:

1. **Install argocd** `kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml`
1. **Install argocd CLI** Downloaded from https://github.com/argoproj/argo-cd/releases/latest
1. Made argocd CLI available on path. 
1. **Changed admin password**
   - **print current pw** `kubectl get pods -n argocd -l app.kubernetes.io/name=argocd-server -o name | cut -d'/' -f 2`
   - **forward port** `kubectl port-forward svc/argocd-server -n argocd 8080:443`
        - _I noticed it would break the connection a couple of times and i needed to execute this command multiple times if strange errors occured._
   - **login** `argocd login localhost:8080`
   - **update** `argocd account update-password`
   - **logout** `argocd logout` _Change of password required a logout if you wanted to do stuff_
   - **login** `argocd login localhost:8080`
1. **Register docker-desktop cluster to deploy apps to** `argocd cluster add docker-desktop`
1. Added the [helm-guestbook](https://github.com/argoproj/argocd-example-apps/tree/master/helm-guestbook) manually in the argocd UI on http://localhost:8080
1. Created public git repo https://github.com/robinvandenbogaard/argo-playground following the [app of apps](https://github.com/argoproj/argocd-example-apps/tree/master/helm-guestbook) pattern using [apps](https://github.com/argoproj/argocd-example-apps/tree/master/apps) as an example
1. The first commit on this repo will show you what was added. From there on out its all stuff to expirement with different settings and mechanisms
1. The sync-application is a default chart created with `helm create sync-application`. Which I then hooked up with the sync-application.yaml in the applications/templates directory.
1. Added the repo to argocd manually using the UI on http://localhost:8080

# Troubles
## During setup
- Port forwarding got messed up a couple of times, needed to re run that command. Some commands failed because connection could not be made. Pay close attention to command and make sure they are ran succesfully!

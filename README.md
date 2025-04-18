# argocd
aws eks update-kubeconfig --region eu-central-1 --name sgo-test --profile ELCASPP-Lab

helm repo add argo https://argoproj.github.io/argo-helm
Only first time : helm show values argo/argo-cd > values.yaml

kubectl create namespace argocd
helm install --values values.yaml argocd argo/argo-cd --namespace argocd [--version 5.9.1]
if needed to update values: helm upgrade --values values.yaml argocd argo/argo-cd --namespace argocd [--version 5.9.1]

cd argocd-install
kubectl apply -f argocd-project.yaml -n argocd
kubectl apply -f argocd-app.yaml -n argocd

kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
kubectl port-forward svc/argocd-server -n argocd 8080:443
Installing HELM
+++++++++++++++
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
helm repo update

Install prometheus
++++++++++++++++++
helm install stable/prometheus \
    --namespace monitoring \
    --name prometheus

Installing Prometheus & Grafana
+++++++++++++++++++++++++++++++

Create configmaps
+++++++++++++++++
kubectl apply -f monitoring/grafana/config.yml
kubectl get configmaps -n monitoring

Install grafana
+++++++++++++++
helm install stable/grafana \
    -f monitoring/grafana/values.yml \
    --namespace monitoring \ 
    --name grafana

Get Grafana password
++++++++++++++++++++
kubectl get secret \
    --namespace monitoring grafana \
    -o jsonpath="{.data.admin-password}" \
    | base64 --decode ; echo


Port forward Grafana Dashboard
++++++++++++++++++++++++++++++
export POD_NAME=$(kubectl get pods --namespace monitoring -l "app=grafana,release=grafana" -o jsonpath="{.items[0].metadata.name}")

kubectl --namespace monitoring port-forward $POD_NAME 3000

Navigate to web browser
+++++++++++++++++++++++
http://localhost:3000

In the left hand menu, choose Dashboards > Manage > + Import

In the Grafana.com dashboard input, add the dashboard ID we want to use: 1860 and click Load

On the next screen select a name for your dashboard and select Prometheus as the datasource for it and 
click Import.


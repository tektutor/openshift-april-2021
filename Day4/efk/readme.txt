#kubectl config set-context --current --namespace=kube-systemm

Setup elasticsearch
+++++++++++++++++++
kubectl create -f kube-logging.yml
kubectl create -f elastic-service.yml
kubectl get services -n kube-logging
kubectl create -f elastic-statefulset.yml
kubectl get pod -n kube-logging -o wide

curl http://XX.XXX.XXX.XXX:9200/

Setup Kibana
++++++++++++
kubectl create -f kibana-service.yml
kubectl create -f kibana-deployment.yml
kubectl get pods -n kube-logging
kubectl port-forward kibana-598vgt546f5-7b9wx 5601:5601 --namespace=kube-logging


Setup Fluent

ServiceAccount - Will allow Fluent to access Kubernetes API in kube-logging namespace
ClusterRole - Grants get,list and watch permissions to Fluent-bit on K8s resources
              like nodes,pods and namespaces
RoleBinding - This integrates ServiceAccount and ClusterRole
ConfigMap -  Has configurations to Input plugin, Parser, Filter, Output
DaemonSet - Ensures one Fluentd runs on  every node to collect metrics

kubectl create -f fluent-bit-service-account.yaml
kubectl create -f fluent-bit-role.yaml
kubectl create -f fluent-bit-role-binding.yaml
kubectl create -f fluent-bit-configmap.yaml
kubectl create -f fluent-bit-ds.yaml

kubectl get ds -n kube-logging


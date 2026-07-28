# Enable the Kubernetes cluster in docker desktop
# clone the repo
git clone https://github.com/gannetejeswar/K8s.git
# Change the folder where your clone the repo go to that folder open cmd
# Run these yamls
kubectl apply -f namespace.yaml
kubectl apply -f configmap.yaml
kubectl apply -f secret.yaml
kubectl apply -f pv.yaml
kubectl apply -f pvc.yaml
kubectl apply -f mysql-deployment.yaml
kubectl apply -f mysql-service.yaml
kubectl apply -f springboot-deployment.yaml
kubectl apply -f springboot-service.yaml
kubectl apply -f ingress.yaml

===============================================
-namespace.yaml

kubectl apply -f namespace.yaml

-List all namespaces

kubectl get namespace

-Check a specific namespace

kubectl get namespace student-management

-View namespace details

kubectl describe namespace student-management

=================================================
-configmap.yaml

kubectl apply -f configmap.yaml

-List all ConfigMaps in your namespace

kubectl get configmap -n student-management

-View the ConfigMap details

kubectl describe configmap student-config -n student-management

-View the ConfigMap in YAML format

kubectl get configmap student-config -n student-management -o yaml

-Check whether your Pod is using the ConfigMap

kubectl get pods -n student-management

========================================================
-secret.yaml

kubectl apply -f secret.yaml

-List all Secrets in the namespace

kubectl get secrets -n student-management

-Check a specific Secret

kubectl get secret student-secret -n student-management

-View Secret details

kubectl describe secret student-secret -n student-management

-View the Secret in YAML

kubectl get secret student-secret -n student-management -o yaml

-Decode the Secret values

kubectl get secret student-secret -n student-management -o jsonpath="{.data.DB_USERNAME}" | base64 --decode  (--username)

kubectl get secret student-secret -n student-management -o jsonpath="{.data.DB_PASSWORD}" | base64 --decode  (--password)

================================================================
-pv.yaml

kubectl apply -f pv.yaml

-List all Persistent Volumes

kubectl get pv

((STATUS meanings
Available → PV is created but not yet bound to a PVC.
Bound → PV is successfully connected to a PVC.
Released → PVC was deleted, but PV still exists.
Failed → There is a problem with the PV.))

-Check a specific PV

kubectl get pv mysql-pv

-View detailed information

kubectl describe pv mysql-pv

-View the PV in YAML

kubectl get pv mysql-pv -o yaml

-Check if the PV is bound to a PVC

kubectl get pvc -n student-management

=======================================================================
-pvc.yaml

kubectl apply -f pvc.yaml

-List all PVCs in your namespace

kubectl get pvc -n student-management

((PVC Status
✅ Bound → PVC is successfully connected to a PV.
⏳ Pending → PVC is waiting for a matching PV.
❌ Lost → The bound PV is no longer available.))

-Check a specific PVC

kubectl get pvc mysql-pvc -n student-management

-View detailed information

kubectl describe pvc mysql-pvc -n student-management

-View the PVC in YAML format

kubectl get pvc mysql-pvc -n student-management -o yaml

-Check whether the MySQL Pod is using the PVC

kubectl describe pod <mysql-pod-name> -n student-management

===============================================================================
-mysql-deployment.yaml
 
 kubectl apply -f mysql-deployment.yaml

-List Deployments

kubectl get deployments -n student-management

or 

kubectl get deploy -n student-management

-Check the MySQL Deployment

kubectl get deployment mysql -n student-management

-Describe the Deployment

kubectl describe deployment mysql -n student-management

-Check the MySQL Pod

kubectl get pods -n student-management

-Check MySQL Logs

kubectl logs deployment/mysql -n student-management

or

kubectl logs <mysql-pod-name> -n student-management

-Check MySQL Service

kubectl get svc -n student-management

==============================================================
-mysql-service.yaml

kubectl apply -f mysql-service.yaml

-List all Services

kubectl get svc -n student-management

or 

kubectl get services -n student-management

-Check only the MySQL Service

kubectl get svc mysql-service -n student-management

-View detailed information

kubectl describe svc mysql-service -n student-management

((✅ Endpoints: 10.x.x.x:3306 → Service is connected to the MySQL Pod.
❌ Endpoints: <none> → The Service is not selecting the MySQL Pod. This usually means the labels in the Deployment and the selector in the Service don't match.))

-Verify the MySQL Pod label

kubectl get pods --show-labels -n student-management

-View the Service YAML

kubectl get svc mysql-service -n student-management -o yaml

===================================================================
-springboot-deployment.yaml

kubectl apply -f springboot-deployment.yaml

-Check all Deployments

kubectl get deployments -n student-management

or

kubectl get deploy -n student-management

-Check only the Spring Boot Deployment

kubectl get deployment student-app -n student-management

-Describe the Deployment

kubectl describe deployment student-app -n student-management

-Check the Pods

kubectl get pods -n student-management

-Check Pod Logs

kubectl logs deployment/student-app -n student-management

or

kubectl logs <pod-name> -n student-management

-Check the Deployment Rollout

kubectl rollout status deployment/student-app -n student-management

-Check the Deployment YAML

kubectl get deployment student-app -n student-management -o yaml

-Check ReplicaSets

kubectl get rs -n student-management

-Check Services

kubectl get svc -n student-management

======================================================================
-springboot-service.yaml

kubectl apply -f springboot-service.yaml

-List all Services

kubectl get svc -n student-management

or

kubectl get services -n student-management

-Check only the Spring Boot Service

kubectl get svc student-service -n student-management

-View detailed information

kubectl describe svc student-service -n student-management

((✅ If you see IP addresses (like 10.1.0.5:8080), the Service is correctly connected to your Spring Boot Pods.
❌ If it shows Endpoints: <none>, the Service is not selecting any Pods))

-Verify the Pod labels

kubectl get pods --show-labels -n student-management

-Open the application

kubectl get svc student-service -n student-management

http://localhost:30080(change based on your port)

-View the Service YAML

kubectl get svc student-service -n student-management -o yaml

=====================================================================================
-ingress.yaml

kubectl apply -f ingress.yaml

-Check all Ingress resources

kubectl get ingress -n student-management

or

kubectl get ing -n student-management

-Check a specific Ingress

kubectl get ingress student-ingress -n student-management


-View detailed information

kubectl describe ingress student-ingress -n student-management

-View the Ingress YAML

kubectl get ingress student-ingress -n student-management -o yaml

-Check if an Ingress Controller is installed
((Creating an Ingress resource alone is not enough. You also need an Ingress Controller (such as NGINX).))

kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml   (for install ingress)

kubectl wait --namespace ingress-nginx \
  --for=condition=ready pod \
  --selector=app.kubernetes.io/component=controller \
  --timeout=300s

kubectl get ns

kubectl get pods -n ingress-nginx

kubectl get svc -n ingress-nginx

kubectl get ingressclass



kubectl get pods -A

or

kubectl get pods -n ingress-nginx

-Check IngressClass

kubectl get ingressclass

-Test the application

http:localhost

========================================================
it is not working then 
-Check if the application is actually listening on port 8080

kubectl port-forward svc/student-service 8080:80 -n student-management


>kubectl cluster-info

=========================================================
-Kubernetes Dashboard (Recommended)
-Step 1: Install Dashboard

kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml

Step 2: Start Dashboard

kubectl proxy

Step 3: Open browser

http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/

Step 4: Login

Create an admin user:

dashboard-admin.yaml

kubectl apply -f dashboard-admin.yaml

Get token:

kubectl -n kubernetes-dashboard create token admin-user

=============================
-change custom url  
-Expose the Dashboard with a NodePort
-You can change the Kubernetes Dashboard Service from ClusterIP to NodePort.

kubectl get svc -n kubernetes-dashboard

kubectl edit svc kubernetes-dashboard-kong-proxy -n kubernetes-dashboard

Change:

type: ClusterIP

to:

type: NodePort

save file

kubectl get svc -n kubernetes-dashboard

-it showing port u can use that (https://localhost:30443)


-code is not open run this

set KUBE_EDITOR=notepad

kubectl edit svc kubernetes-dashboard -n kubernetes-dashboard  (by using this we can't save so follow below steps)

kubectl get svc -n kubernetes-dashboard -o yaml > dashboard-service.yaml

((This creates a new file named dashboard-service.yaml in your current folder.))

code dashboard-service.yaml (open in vscode)

kubectl apply -f dashboard-service.yaml

kubectl port-forward -n kubernetes-dashboard service/kubernetes-dashboard 8443:443  (but https is not work some systems)

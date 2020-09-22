# helm-monitoring

# Install Prometheus using helm:

# Update dependencies for prometheus
    cd  prometheus
    helm dependency update
    cd ../
# Create prometheus namespace:
	kubectl create namespace prometheus
# Install prometheus using helm:
	helm install prometheus prometheus \
		--namespace prometheus \
		--set alertmanager.persistentVolume.storageClass="gp2" \
		--set server.persistentVolume.storageClass="gp2" 
# Check details:
	kubectl get all -n prometheus
# Change service type as LoadBalancer from ClusterIP:
	kubectl edit svc prometheus-server -n prometheus
![image](https://user-images.githubusercontent.com/68885738/92363557-486b0c80-f10f-11ea-9132-d2ff37513a1f.png)
# Check service of prometheus
    kubectl get svc -n prometheus
# Check promethus in webUI:
    http://a4e29142a2b51463fac7d3238106d32b-896983688.us-east-1.elb.amazonaws.com/graph
![image](https://user-images.githubusercontent.com/68885738/92363763-a566c280-f10f-11ea-91cd-a5afe7aa2b20.png)
# Install Grafana using helm:

# Create file with grafana.yaml name:
	datasources:
	  datasources.yaml:
	    apiVersion: 1
	    datasources:
	    - name: Prometheus
	      type: prometheus
	      url: http://prometheus-server.prometheus.svc.cluster.local
	      access: proxy
	      isDefault: true
# Update dependencies for grafana
	cd grafana
	helm dependency update
	cd ../
# Create namespe with grafana:
	kubectl create namespace grafana 
# Install grafana using below command:
	helm install grafana grafana \
		--namespace  grafana \
		--set persistence.storageClassName="gp2" \
		--set persistence.enabled=true \
		--set adminPassword='admin123' \
		--values grafana.yaml \
		--set service.type=LoadBalancer
# Check details:
    kubectl get all -n grafana
# Check service of grafana
    kubectl get svc -n grafana
![image](https://user-images.githubusercontent.com/68885738/92364049-16a67580-f110-11ea-9105-85b048c1acc5.png)
# Credentials:
    username: admin
    password: admin123   
# Cluster Monitoring Dashboard
For creating a dashboard to monitor the cluster:
* Click '+' button on left panel and select 'Import'.
* Enter 3119 dashboard id under Grafana.com Dashboard.
* Click 'Load'.
* Select 'Prometheus' as the endpoint under prometheus data sources drop down.
* Click 'Import'.
This will show monitoring dashboard for all cluster nodes:

![image](https://user-images.githubusercontent.com/68885738/93885898-64bb9b80-fd02-11ea-80c0-d13dc292b7de.png)
# Pods Monitoring Dashboard
For creating a dashboard to monitor all the pods:
* Click '+' button on left panel and select ‘Import’.
* Enter 6417 dashboard id under Grafana.com Dashboard.
* Click 'Load'.
* Enter Kubernetes Pods Monitoring as the Dashboard name.
* Click change to set the Unique identifier (uid).
* Select 'Prometheus' as the endpoint under prometheus data sources drop down.s
* Click 'Import'.

![image](https://user-images.githubusercontent.com/68885738/93886127-acdabe00-fd02-11ea-8a00-4ba187bc3480.png)

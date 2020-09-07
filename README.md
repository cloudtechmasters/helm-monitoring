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

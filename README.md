# voting-app

This app is part of an assignment for the BTH master

The steps to run the app are the following:
1) Clone this repository. In Linux just use the `git clone` command
2) Install and start minikube. Add the service Ingress
3) Create all deployment and services .yaml files in this way: `kubectl create -f redis-deploy.yaml` with the output `deployment.apps/redis-deploy created`.
4) Check all deployments and services are ready by executing `kubectl get pods,svc` with the following result
![Screenshot from 2023-11-07 08-51-27](https://github.com/raul-parada/voting-app/assets/8438920/eeb8dfb9-e986-40c3-a9d3-b79a73a510b7)

5) Check the IPs and port to access the apps: `minikube service voting-service --url` with output `http://192.168.59.100:30004`
6) The following screens 

![Screenshot from 2023-11-07 09-50-54](https://github.com/raul-parada/voting-app/assets/8438920/a3d0b17b-ab66-4f36-bfe4-e1fcd207d817)


![Screenshot from 2023-11-07 09-50-23](https://github.com/raul-parada/voting-app/assets/8438920/0f6f46c8-fb55-4f5a-921e-4335462e5789)

7) We can access to the microservices externally by applying the ingress files with `kubectl apply -f voting-ingress.yaml` and output
`ingress.networking.k8s.io/voting-ingress configured`. We can check the ingress services are running by executing `kubectl get ingress`

![Screenshot from 2023-11-07 10-03-22](https://github.com/raul-parada/voting-app/assets/8438920/d7466bb2-6dba-4027-9dbb-685feec823d5)

8) By executing the following command we can assure the microservices are accessible `curl --resolve result.example.com:80:192.168.59.100 http://result.example.com`

![Screenshot from 2023-11-07 10-05-21](https://github.com/raul-parada/voting-app/assets/8438920/42b21fd1-cd6a-41da-b459-b7edc1d46fa2)

9) We must use minikube tunnel to check the current services available outside:
```
Status:	
	machine: minikube
	pid: 3203
	route: 10.96.0.0/12 -> 192.168.59.100
	minikube: Running
	services: []
    errors: 
		minikube: no errors
		router: no errors
		loadbalancer emulator: no errors`
```

10) As can be seen, no services are displayed within services. We have to apply changes in the already created result and voting services .yaml files. This change is to modify the NodePort to LoadBalancer. We `kubectl apply -f <name service file>` and if we check the tunnel output the services appear:
```
Status:	
	machine: minikube
	pid: 3203
	route: 10.96.0.0/12 -> 192.168.59.100
	minikube: Running
	services: [result-service]
    errors: 
		minikube: no errors
		router: no errors
		loadbalancer emulator: no errors
Status:	
	machine: minikube
	pid: 3203
	route: 10.96.0.0/12 -> 192.168.59.100
	minikube: Running
	services: [result-service, voting-service]
    errors: 
		minikube: no errors
		router: no errors
		loadbalancer emulator: no errors
```


12) We check the external IPs and the ports to access outside:

```
$kubectl describe services result-service
Name:                     result-service
Namespace:                default
Labels:                   app=demo-voting-app
                          name=result-service
Annotations:              <none>
Selector:                 app=demo-voting-app,name=result-app-pod
Type:                     LoadBalancer
IP Family Policy:         SingleStack
IP Families:              IPv4
IP:                       10.110.1.59
IPs:                      10.110.1.59
LoadBalancer Ingress:     10.110.1.59
Port:                     <unset>  80/TCP
TargetPort:               80/TCP
NodePort:                 <unset>  30005/TCP
Endpoints:                10.244.0.18:80
Session Affinity:         None
External Traffic Policy:  Cluster
Events:                   <none>
$kubectl describe services voting-service
Name:                     voting-service
Namespace:                default
Labels:                   app=demo-voting-app
                          name=voting-service
Annotations:              <none>
Selector:                 app=demo-voting-app,name=voting-app-pod
Type:                     LoadBalancer
IP Family Policy:         SingleStack
IP Families:              IPv4
IP:                       10.104.170.27
IPs:                      10.104.170.27
LoadBalancer Ingress:     10.104.170.27
Port:                     <unset>  80/TCP
TargetPort:               80/TCP
NodePort:                 <unset>  30004/TCP
Endpoints:                10.244.0.21:80
Session Affinity:         None
External Traffic Policy:  Cluster
Events:                   <none>
```



12) If we enter to a browser and enter the LoadBalancer Ingress IP with the port. In this case: 10.110.1.59:80 (result-service) and 10.104.170.27 (voting-service). We get the following result:


![Screenshot from 2023-11-09 21-28-36](https://github.com/raul-parada/voting-app/assets/8438920/47248575-c94e-470b-809b-fdabd273f519)


  

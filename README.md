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

8) By executing the following command we can assure the microservices are accessible from outside `curl --resolve result.example.com:80:192.168.59.100 http://result.example.com`

![Screenshot from 2023-11-07 10-05-21](https://github.com/raul-parada/voting-app/assets/8438920/42b21fd1-cd6a-41da-b459-b7edc1d46fa2)

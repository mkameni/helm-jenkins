### Running Jenkins using helm

#### Demarrage de l'environement minikube :
```shell
  $ minikube start
  $ minikube dashboard
```

#### Creation du namespace :
```shell
  $ kubectl create -f deploy/volume.yaml
```

#### Creation du volume persistant :
```shell
  $ mkdir ${PWD}/data
  $ kubectl create -f deploy/volume.yaml
  $ minikube mount ${PWD}/data:/var/jenkins_home
```

#### Deployement (installation) de jenkins :
```shell
  $ helm install --name jenkins-dev-01 -f helm/values_2.yaml stable/jenkins --namespace jenkins-dev-01
  $ minikube service jenkins-dev-01 --namespace jenkins-dev-01
```

#### Recuperation du mot de passe admin :
```shell
  $ printf $(kubectl get secret --namespace jenkins-dev-01 jenkins-dev-01 -o jsonpath="{.data.jenkins-admin-password}" | base64 --decode);echo
```

#### Suppression de jenkins :
```shell
  $ helm delete --purge jenkins-dev-01
  $ kubectl delete -f deploy/volume.yaml
  $ kubectl delete -f deploy/namespace.yaml
```

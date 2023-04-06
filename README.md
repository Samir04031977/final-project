# Deployer 3 API et les faire communiquer entre elles

### Dockerizer les 3 API

#### Construire un Docker-compose 

Pour Dockerizer les 3 API, il faut, construire un docker-compose dans un terminal ouvert à la racine du projet, exécuter les commandes ci-dessous: 

```bash
docker docker compose up -d
```
Pour pousser les images  sur DockerHub, il faut, après s'être logué avec `docker login`, exécuter la commande ci-dessous: 

```bash
docker push nom-repo/nom-image
```

---

Pour pouvoir tester les endpoints (les chemins à attaquer via le client RESt), il est nécessaire, dans notre cas, de faire des appels REST de type POST tel que: 

* Pour tester l'utilisation de l'API concernant les utilisateurs, on peut faire un POST à l'endpoint http://localhost:8080/signup ou http://localhost:8080/login. La requête doit comporter un Bearer Token ayant pour valeur `abc` et porter comme corps un JSON de ce type: 

```json
{
  "email": "test@example.com",
  "password": "password"
}
```

Dans le cas où l'API fonctionne, on aura une réponse ayant pour code de statut un **2xx** une réponse sous la forme d'un JSON, confirmant le bon fonctionnement de l'API. 

* Pour tester l'API des tâches, il nous faut via le client REST, réaliser un POST vers http://localhost:8000/tasks avec un Bearer token ayant pour valeur `abc` et comme corps de la requête un JSON ayant pour structure: 

```json
{
  "text": "Valeur de text",
  "title": "Valeur de title"
}
```

Le bon fonctionnement de l'API sera confirmé par un code de status **2xx** et une réponse sous la forme d'un JSON comme pour l'API précédente.


### Créer et paramétrer le cluster Kubernetes

avant de faire communiquer les 3 API entre elles, il faut deja créer un cluster avec minikube, pour cela on va utiliser la commande:

```bash
minikube start --driver docker
```

Puis il nous faudra appliquer nos fichiers de ressources via la commande ci-dessous: 

```bash
kubectl apply -f .\ressources-files\
```

* Pour tester l'utilisation de l'API concernant les utilisateurs, faut d'abord lancer le service la concernant avec la commande ci dessous :
```bash
minikube service exo-05-service
```

Ensuite faire un POST à l'endpoint http://ip:8080/signup ou http://ip:8080/login. La requête doit comporter un Bearer Token ayant pour valeur `abc` et porter comme corps un JSON de ce type: 

```json
{
  "email": "test@example.com",
  "password": "password"
}
```
Dans le cas où l'API fonctionne, on aura une réponse ayant pour code de statut un **2xx** une réponse sous la forme d'un JSON, confirmant le bon fonctionnement de l'API. 

* Pour tester l'API des tâches, il nous faut via le client REST, réaliser un POST vers http://ip:8000/tasks avec un Bearer token ayant pour valeur `abc` et comme corps de la requête un JSON ayant pour structure: 

```json
{
  "text": "Valeur de text",
  "title": "Valeur de title"
}
```

Le bon fonctionnement de l'API sera confirmé par un code de status **2xx** et une réponse sous la forme d'un JSON comme pour l'API précédente.





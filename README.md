# Fake Shop


## Variável de Ambiente

### Postgre
DB_HOST	=> Host do banco de dados PostgreSQL.

DB_USER => Nome do usuário do banco de dados PostgreSQL.

DB_PASSWORD	=> Senha do usuário do banco de dados PostgreSQL.

DB_NAME	=>	Nome do banco de dados PostgreSQL.

DB_PORT	=>	Porta de conexão com o banco de dados PostgreSQL.

### FLASK_APP
```
#!/bin/bash
export FLASK_APP=index.py
python -m flask db upgrade
python -m gunicorn --bind 0.0.0.0:5000 index:app
```

## Comandos K3D

Agents =  WorkNodes

Servers = Control Pane

Exemplos para criar o cluster:
```
k3d cluster create --servers 1 --agents 3

k3d cluster create --servers 1 --agents 3 -p "5000:30000@loadbalancer"
```

Listar o cluster:
```
k3d cluster list
```

Deletar o cluster:
```
k3d cluster delete
```


## Docker

Iniciar o serviço no wsl:
```
sudo service docker start
```

Construir a imagem:
```
docker build -t fabiocaettano74/fake-shop-desafio:v1 .
```

Realizar o upload para o docker hub:
```
dokcer push fabiocaettano74/fake-shop-desafio:v1 .
```


## Comandos Kubectl

Listar objetos:

``` 
kubectl get nodes
kubectl get pod
kubectl get all
```

Listar os recursos para verificar 
```
kubectl api-resources
```

Criar os objetos do cluster kubernetes:
```
kubectl apply -f k8s/deployment.yaml
```

Destruir os objetos do cluster kubernetes:
```
kubectl delete -f k8s/deployment.yaml
```

Listar Logs:
```
kubectl logs pod/postgre-69f8d54cc-qmmts
``` 

Expor aplicação:
```
kubectl port-forward pod/postgre-69f8d54cc-qmmts 5432:5432
kubectl port-forward service/fakeshop 5000:80
```

Retornar versão anterior:
```
kubectl rollout history deployment fakeshop
kubectl rollout undo deployment fakeshop
```

## Acessar aplicação

### Para acessar aplicação localmente (Node Port)

É necessário expor aplicação através do port-forward e ajustar o service do manifesto que criar os objetos do kubernetes:

Comando:

```
kubectl port-forward service/fakeshop 5000:80
```

Service:

```
apiVersion: v1
kind: Service
metadata:
  name: fakeshop
spec:
  type: NodePort
  selector:
    app: fakeshop
  ports:
  - port: 80
    targetPort: 5000
    nodePort: 30000
```

### Para acessar aplicação externamente (Load Balancer)

Necessário criar o cluster Kubernetes em um serviço de nuvem.
Fazer o download do arquivo "config".
Este arquivo deve ser colocado na pasta "~/.kube".
Realizar o deployment, o service abaixo com o tipo LoadBalancer que irá reservar um IP.

Service:

``` service
apiVersion: v1
kind: Service
metadata:
  name: fakeshop
spec:
  type: LoadBalancer
  selector:
    app: fakeshop
  ports:
  - port: 80
    targetPort: 5000
```
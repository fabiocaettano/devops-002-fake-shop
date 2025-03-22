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
k3d cluster create --servers 1 --agents 3

k3d cluster create --servers 1 --agents 3 -p "5000:30000@loadbalancer"

k3d cluster list

k3d cluster delete


## Doccker

sudo service docker start

docker build -t fabiocaettano74/fake-shop-desafio:v1 .

dokcer push fabiocaettano74/fake-shop-desafio:v1 .

## Comandos Kubectl

kubectl get nodes

kubectl api-resources

kubectl apply -f k8s/deployment.yaml

kubectl delete -f k8s/deployment.yaml

kubectl get pod

kubectl get all

kubectl logs pod/postgre-69f8d54cc-qmmts

kubectl port-forward pod/postgre-69f8d54cc-qmmts 5432:5432

kubectl port-forward service/fakeshop 5000:80


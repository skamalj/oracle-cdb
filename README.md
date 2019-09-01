# Creating oracle CDB/PDB instance on GKE
## Create GKE cluster 
```
gcloud container clusters create oraclecdb --num-nodes 2 --machine-type n1-highmem-2 --region asia-south1-a
```
## Clone the repository
## Enable oracle docker hub registry for your account 
Follow the instructions [over here](https://hub.docker.com/_/oracle-database-enterprise-edition)
## Login to your docker account
docker login
## Create kubernetes secret for docker registry 
Follow the instruction on [this page]( https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/) to create kubernetes secret for docker registry. Command you will use is, 
```
kubectl create secret generic regcred \
    --from-file=.dockerconfigjson=<path/to/.docker/config.json> \
    --type=kubernetes.io/dockerconfigjson
```
## Now create oracle CDB, this will take 4-5 mins.
```
kubectl apply -f cbd.yaml
```
## Get service Nodeport using below command
```
kubectl get svc 
```

## Now connect using any sqlclient, I used sqldeveloper
```
IP = get kubernetes public IP from console
port = port you egt from above command , will be in range 3xxxx
userid = sys
password = Oradoc_db1
service_name = MYCDB.kamalsblog.com
```

## You can also connect using port-forward on svc, say using port 1521
```
In that case 
IP = localhost
port = 1521
user/pass same as above.
service_name = MYCDB.kamalsblog.com
```
[This page](https://hub.docker.com/_/oracle-database-enterprise-edition) details all the connectivity and configuration options available.

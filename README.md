# Notas generales K8S

Kubernetes es declarativo

Kubernetes cuenta con un panel de control y con los nodos. Cada uno de los nodos estará ejecutando el agente de kubelet y kube-proxy, por otra parte, ETCD es la base de datos de K8S.

En la actualidad es fácil instalar K8S en cualquier parte. A continucación se muestra el proceso de instalación de K8S en un ambiente local de LINUX (Linux Mint)

### Instalación entorno de K8S

1.Instalar Kubectl. Con el siguiente comando puedes comprobar la versión que tienes instalada de Kubectl
    
    kubectl version --client=true
    
2.Para conectar la herramienta o el cliente Kubectl a un cluster de Kubernetes ocupas un archivo llamado **kube.config**, es donde estan declarados todos tus contextos (es una combinación de la URL para conectarte). Los clusters puedes estar creados en ambientes de nube (AWS, Azure, Digital Ocean...) o en un ambiente local (Minikube).

### Comandos generales de K8S
3.Comando para obtener los nodos de kubernetes que se estan ejecutando en el cluster.

    kubectl get nodes

4.Comando para mostrar los **contextos** que tienes en tu archivo kube.config.

    kubectl config get-contexts

*Existe una herramienta llamada **Lens** que es una interfaz gráfica a kubectl*

5.Namespaces es una manera de separar el trafico de alguna forma o los recursos, con el siguiente comando puedes ver los **namespaces** que tienes creados dentro del cluster.

    kubectl get ns

6.Un **POD** es un set de contenedores que puede estar basado en X contenedores, generalmente se ejecuta un POD por cada contenedor

    kubectl -n NOMBRE_NAMESPACE get pods
    podemos poner la opción -o wide y nos muestra mas información de los pods
    kubectl -n NOMBRE_NAMESPACE get pods -o wide

7.Puedes ejecutar un **POD** directamente de un archivo manifiesto yaml con el siguiente comando

    kubectl apply -n NAMESPACE -f NOMBRE_ARCHIVO

    *Si no le especificas un namespaces, tomará el default*

8.Podemos ejecutar **comandos dentro de los PODS** (contenedores a elegir) con el siguiente comando:

    kubectl exec -n NAMESPACE -it NOMBRE_CONTENEDOR -- COMMANDO
    kubectl exec -n default -it nginx -- sh  INGRESA AL CONTENEDOR POR MEDIO DE SH

9.Para **eliminar un POD** directamente se puede hacer con el siguiente comando

    kubectl -n NAMESPACE delete pod NOMBRE_POD
    kubectl -n default delete pod nginx

*NO ES RECOMENDABLE DESPLEGAR SOLO PODS, PARA ESO EXISTEN LOS **DEPLOYMENTS** EN K8S QUE SON TEMPLATES PARA CREAR PODS*

10.Seguimos manejando la misma estructura para ejecutar un manifiesto, pero en este caso de un deployment

    kubectl apply -n NAMESPACE -f NOMBRE_ARCHIVO
    kubectl apply -n default -f deployment-gninx-avanzado.yaml
    Eliminar un deployment
    kubectl delete -f NOMBRE_ARCHIVO_DEPLOYMENT_LANZADO

*EXISTE OTRA MANERA DE DESPLEGAR PODS, **DAEMONSET** ES LA MANERA DE DESPLEGAR PODS EN TODOS LOS NODOS DEL CLUSTER, NO TIENE EL ATRIBUTO REPLICAS. SE UTILIZA MAS PARA SERVICIOS DE MONITOREO O CAPTURAR LOGS*

11.Seguimos manejando la misma estructura para ejecutar un manifiesto, pero en este caso de un **daemonset**

    kubectl apply -n NAMESPACE -f NOMBRE_ARCHIVO
    kubectl apply -n default -f daemonset-gninx-avanzado.yaml

*TAMBIÉN PODEMOS CREAR PODS CON UN TIPO **STATEFULSET**, ES UNA MANERA DE CREAR PODS COMO EN UN DEPLOYMENT PERO PUEDE TENER UN **VOLUMEN** (SIMILAR A DOCKER, ES UN DISCO ATADO AL POD), ES MUY UTIL PARA BD. EL VOLUMEN PUEDE PERSISTIR A PESAR DE LA ELIMINACIÓN DEL POD ATACHADO*

12.Seguimos manejando la misma estructura para ejecutar un manifiesto, pero en este caso de un **statefulset**
    
    kubectl apply -n NAMESPACE -f NOMBRE_ARCHIVO

13.Podemos describir los PODS mas a detalle con el siguiente comando
    
    kubectl describe pod NOMBRE_POD

14.Mostrar **PVC** (Persisten Volume Claims)
    
    kubectl -n NAMESPACE get pvc
    kubectl -n NAMESPACE describe PVC

**NETWORKING EN KUBERNETES**

15.Cada **POD** tiene su propia dirección IP

**SERVICIOS EN KUBERNETES**

Es una forma de contactar aplicaciones, un set de PODS, de un POD hacia otro o desde afuera de cluster.
Existen 3 tipos de servicios en K8S: 
    ClusterIP.- Es una IP fija que esta dentro del cluster, actua como un mini load balancer 
    Node Port.- Crea un puerto en cada nodo, similar
    Load Balancer.- Mas atado a la nube, crea un balanceador de carga en nuestro cliente de nube

*CLUSTER IP*
16.Mostramos todo lo que tiene nuestro namespace

    kubectl -n NAMESPACE get all
    kubectl -n default get all

17.Si queremos saber que dirección IP tiene cada POD dentro de un servicio de CLUSTER IP, lo hacemos con el siguiente comando

    kubectl -n NAMESPACE describe service NOMBRE_SERVICE
    kubectl -n default describe svc hello

o también lo podemos ver obteniendo todos los PODS con el comando general.

    kubectl -n NAMESPACE get pods -o wide

conectandonos al contenedor de ubuntu

    kubectl -n NAMESPACE exec -it CONTAINER -- bash

*NODE PORT*
18.Es una manera de exponer todo el nodo completo al puerto que se defina en el servicio NODE PORT, de esta forma podríamos exponer todo el nodo al internet.

*LOAD BALANCER*
19.

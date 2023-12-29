# dockers

Repo para almacenar dockerFiles de utilidad

Para crear una imagen nos situamos en el directorio del Dockerfile

```shell
docker build -t jenkins/
```

Y para ejecutar el docker

```shell
docker run -p 80:80 -p 50000:50000 --name jenkinsblue jenkins/blueocean_ci
```

## sonarQube

Descargar imagen sonar

```shell
docker pull sonarqube
```

Crear contenedor de sonar

```shell
docker run -d --name sonarqube -p 9000:9000 -p 9092:9092 sonarqube
```

crear red virtual

```shell
docker network create jenkins_sonarqube
```

verificar redes

```shell
docker network ls
```

conectar los contenedores a la red virtual

```shell
docker network connect jenkins_sonarqube sonarqube
docker network connect jenkins_sonarqube jenkinsblue
```

Ver los detalles de un contenedor

```shell
docker container inspect sonarqube
```

Parametros de la configuración del paso referente a sonar en jenkins

```shell
# task to run
scan

# Propiedades usadas en la configuracion del pipeline para añadir el paso de sonarqube
sonar.login=token-generado-en-sonarqube
sonar.projectKey=sonarqube
sonar.sources=billing/src/main/java
sonar.java.binaries=billing/target/classes

# en Additional arguments, esto habilita el debug
-X
```

### Configurar el agente de docker para construir la imagen en el pipeline

plugin que debe ser instalado

```shell
CloudBees Docker Build and Publish
```

nombre de imagen

```shell

cuentadockerhub/billingapp-backend
```

url del agente

```shell
tcp://172.17.0.1:2375
```

contexto para l aimagen del backend

```shell
billing/
```

argumentos adicionales

```shell
--build-arg  JAR_FILE=target/*.jar
```

ruta del fichero que se debe editar en el host para exponer el api del daemon de docker usar nano o vi con sudo para poder guardar

```shell
/lib/systemd/system/docker.service
```

ajustar el contenido de la linea para que quede como esta

```shell
ExecStart=/usr/bin/dockerd -H fd:// -H=tcp://0.0.0.0:2375
```

Reiniciar servicios

```shell
sudo systemctl daemon-reload

sudo service docker restart
```

verificar y/o reiniciar contenedores de docker (docker ps -> ver contendores activos docker ps -a -> ver todos)

Comprobar que el api es accesible mediante el protocolo http cone l sigueinte comando o en un navegador

```shell
curl http://localhost:2375/images/json
```

Hacer puente entre el contenedor de jenkis y el host local para acceder al docker daemon mediante tcp

```shell
ip route show default | awk '/default/ {print $3}'
```

despues

```shell
ip a
```

buscar la red de docker 01 y usarla en lugar de localhost, en la configuracion del plugin en el pipeline deberia ser (172.17.0.1) o similar

#### Configurar la integracion con kubernetes

conectarse como root en jenkis e installar el kubectl

```shell
docker exec -it --user=root jenkins /bin/bash

# o tambien en caso de que se llame jenkinsblue
docker exec -it --user=root jenkinsblue bash
```

Instalar kubectl en linux

```shell
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl" && chmod +x kubectl


# y movemos los binarios
 mv ./kubectl /usr/local/bin/kubectl

# y verificamos
kubectl version --client
```

listar las redes

```shell
docker network ls
```

conectar jenkis a la red de minikube (docker network ls)

```shell
docker network connect minikube jenkins
```

para desconectar de la red

```shell
docker network disconnect minikube jenkins
```

plugin que debe ser instalado
Kubernetes plugin

aplicar la configuración (ojo, hay que hacer un minikube start primero para activar el cluster)

```shell
kubectl apply -f jenkins-account.yaml
```

ver la configuracion de minikube

```shell
kubectl config view
```

consultar los server account

```shell
kubectl --namespace default get serviceaccount
```

vel el detalle del server account

```shell
kubectl --namespace default get serviceaccount jenkins -o yaml
```

obtener el token del server account (esto se ver lanzado el comando anterior en secret)

```shell
kubectl describe secrets/jenkins-token-rk2mg
```

## Prometeus y grafana

1. Namespaces: Clusters virtuales en un mismo cluster fisico (separacion logic ade clusters)
   https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/

2. verificar namespaces

   ```shell
   kubectl get namespace
   ```

3. Crear el namespace si no existe

   ```shell
   kubectl create namespace monitoring
   ```

   devops-udemy-11-p

4. Crear role de monitorizacion
   Referencia Authoriation
   https://kubernetes.io/docs/reference/access-authn-authz/rbac/

   ```shell
   kubectl apply -f moniring-role.yaml
   ```

5. Crear fichero de configuracion para externalizar la configuracion de prometheus(independiente del ciclo de vida del contenedor)

   ```shell
   kubectl apply -f configmap-prometheus.yaml
   ```

6. Crear el contenedor y el servicio de prometheus (el contenedor)

   ```shell
   kubectl apply -f deployment-prometheus.yaml
   ```

7. verificar los pods del namespace monitoring

   ```shell
   kubectl get all --namespace=monitoring
   ```

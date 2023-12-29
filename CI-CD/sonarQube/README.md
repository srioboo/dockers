# sonarQube

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

# sonarQube

Descargar imagen sonar
```
docker pull sonarqube
```

Crear contenedor de sonar
```
docker run -d --name sonarqube -p 9000:9000 -p 9092:9092 sonarqube
```

crear red virtual
```
docker network create jenkins_sonarqube
```

verificar redes
```
docker network ls
```

conectar los contenedores a la red virtual
```
docker network connect jenkins_sonarqube sonarqube
docker network connect jenkins_sonarqube jenkinsblue 
```

Ver los detalles de un contenedor 
```
docker container inspect sonarqube 
```

Parametros de la configuración del paso referente a sonar en jenkins
```
# task to run 
scan

# Propiedades usadas en la configuracion del pipeline para añadir el paso de sonarqube
sonar.login=token-generado-en-sonarqube
sonar.projectKey=sonarqube
sonar.sources=billing/src/main/java
sonar.java.binaries=billing/target/classes

# en Additional arguments, esto habilita el debug
-X

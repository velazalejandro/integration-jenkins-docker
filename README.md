# integration-jenkins-docker
# Título
Integración de Jenkins en Docker

## Comenzando 🚀
_Estas instrucciones te permiten obtener una copia del proyecto en funcionamiento en tu máquina local para propósitos de desarrollo y pruebas._

### Pre-requisitos 📋
_Que cosas necesitas para instalar el software y como instalarlas_
- Manejar la consola de CMD para la ejecución de comandos.
- Docker Desktop.


### Instalación 🔧 Pruebas ⚙️ y Despliegues 📦
Integración de Jenkins en Docker:
Si nos vamos al Docker Hub veremos que tenemos multitud de tags para la imagen de jenkins/jenkins
https://hub.docker.com/_/jenkins

Descargar la imagen jenkins al Docker Host:
Primero tenemos que registrarnos con una cuenta de DockerHub para poder descargar la imagen de Jenkins.

1. Ejecutar la plataforma Docker de manera local:
En Docker Desktop - settings - docker engine - Añadimos la línea "experimental": true para que nos funcione la descarga de la imagen en el CMD.
<img width="1198" height="431" alt="image" src="https://github.com/user-attachments/assets/5f4d2cf7-bc78-405f-b010-682bf4a671c5" />


3. Dirigirse a https://hub.docker.com/ y buscar la imagen oficial de la herramienta Jenkins para ejecutar dentro de un contenedor Docker.

4. Para descargar nuestra imagen jenkins/jenkins que hayamos elegido lo haremos con el comando docker pull tal que así:
docker pull jenkins/jenkins:lts
<img width="896" height="451" alt="image" src="https://github.com/user-attachments/assets/76e5953d-90c8-4455-907a-e578e6b7483b" />


6. Verificamos que la imagen se haya descargado correctamente:
docker images

7. Ejecutamos jenkins como contenedor Docker exponiéndolo en el puerto 8080 del ordenador local:
docker run -p 32769 -p 50000:50000 -v /var/jenkins_home jenkins/jenkins:lts
docker run -p 8080:8080 -p 50000:50000 -v /var/jenkins_home jenkins/jenkins:lts
El comando con el que funciona y arranca jenkins con docker:
Docker run –it –p 8080:8080 jenkins/jenkins:lts
<img width="419" height="22" alt="image" src="https://github.com/user-attachments/assets/e433be1c-3709-4a3a-b3f9-ae078d2eabc8" />

Copiamos la contraseña del administrador para acceder a jenkins.
<img width="976" height="447" alt="image" src="https://github.com/user-attachments/assets/a74444c8-9edc-48f0-81ce-5e36be6402a3" />


9. Verificamos que se pueda ingresar al contenedor a través del navegador web de preferencia.
La imagen jenkins lts funciona perfectamente – In Use.
Abrimos la imagen del puerto 8080:8080
Nos aparece en el navegador localhost:8080/login y comienza el proceso de instalación de Jenkins.
Copiamos la clave del administrador en Administrator password.
En la siguiente ventana nos aparece la instalación de plugions - Seleccionamos la opción Install suggested plugins. Una vez hecho el paso, aparece Getting Started ya comienza la instalación.
Una vez terminada la instalación, nos pide crear un primer usuario Admin - Create First Admin User.
Una vez creado el usuario, aparece la Configuración de la instancia. Añadimos la URL de Jenkins con el puerto asociado: http://localhost:8080/

10. Cancelamos la ejecución actual y ahora hazlo asignando un volumen dentro del ordenador local para guardar las configuraciones que hagamos en la herramienta y no se pierdan a medida que ejecutamos el contenedor.
docker run -p 8080:8080 -p 50000:50000 -v /var/jenkins_home jenkins/jenkins
docker run -p 32769 -p 50000:50000 -v /var/jenkins_home jenkins/jenkins

11. Para ingresar a Jenkins por primera vez es necesario tener a la mano la contraseña de administrador que se muestra en la consola de comandos que se muestra a medida que el contenedor se inicia. También la podrán encontrar posteriormente en el archivo que se crea en el volumen llamado /your/home/secrets/initialAdminPassword.
Ejecutamos el comando: docker exec keen_herschel cat
/var/jenkins_home/secrets/initialAdminPassword
Keen_herschel es el nombre del contenedor que se ha creado en la imagen jenkins/jenkins:lts
/var/jenkins_home/secrets/initialAdminPassword es el fichero donde se encuentra el password inicial para jenkins.
De esta forma puedes guardar el password en una variable de entorno y usarlo en un script, para acceder desde otra aplicación usando la API, etc.
JENKINS_InitialPassword=$(docker exec keen_herschel /var/jenkins_home/secrets/initialAdminPassword)

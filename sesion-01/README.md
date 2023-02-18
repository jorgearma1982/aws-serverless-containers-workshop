## Sesión 01: Introducción a los contenedores en ECS: EC2 y fargate

### Introducción

#### Objetivos

Los objetivos de esta sesión son:

* Reconocer el concepto de contenedores
* Construir una pagina web de ejemplo que corra meditante estos servicios

En especifico:

* Reconocer el concepto de contenedores
* Aprender la base del funcionamiento de los contenedores
* Entender las diferencias principales con las máquinas virtuales
* Identificar qué es docker y cuáles son sus funciones

### Antecedentes

En el prework lograr
Los objetivos de esta sesión de work son:

* Practicar con Docker
* Comprender el funcionamiento de los contenedores en la nube
* Descrubrir el servicio ECS y sus componentes
* Lanzar un web service completamente administrador desde una imagen almacenada en ECR

## Practicar con docker


### Actividades


#### Construir un Clúster ECS

En esta actividad vamos a realizar las siguientes tareas:

* Crear un clúster ECS
* Crear una definición de tarea
* Crear un servicio
* Permitir el acceso al servicio
* Validar el servicio

##### Crear una definición de tarea

Ya que hemos creado el clúster ECS, debemos crear una definición de tarea, para esto hacemos clic
en `Crear nueva tarea`.

TODO.

En las opciones de `Contenedor` asegurese de usar los nombres del repositorio y el URI de la imagen
tal cual como se creó en la actividad anterior. Debemos usar la opción de `Contenedor esencial` para
esta actividad.


##### Crear un servicio

Una vez que hemos creado la definición de la tarea en el cluster debemos hacer la implementación
como un servicio, para eso debe hacer clic en el botón `Implementar` y selección `Crear servicio`.

TODO: Describir las opciones.

**IMPORTANTE:** Debe asegurarse que en las opciones de `Rol de tarea` y `Rol de ejecución de tareas`
use `ecsTaskExecutionRole` para garantizar la correcta implementación.

##### Dar acceso al servicio

Por default, el servidor donde se ejecuta la carga de trabajo que acabamos de dar de alta solo
permite el acceso desde el mismo VPC, por lo que si se intenta acceder al servicio usando
la IP publica asociada a la tarea desde nuestra conexión de casa no podrémos entrar.

TODO: Foto del security group predefinido donde se ve un origin limitado.

Debe editar el security group asociado a la red donde se desplegó el cluster y agregar una regla
de entrada que permita el acceso al servicio en el puerto `TCP/5000` a un origen definido.

TODO: Foto de regla de entrada con origen `0.0.0.0/0`.

##### Validación

Después que se creó el definition task, el servicio y de que se dió el acceso en el security group
debemos obtener la dirección IP donde se publica el servicio.

Para esto vamos a el cluster, luego en las tareas, seleccionamos la tearea que definimos y luego
vamos a la pestaña `Redes` en donde aparece la dirección IP Externa por la que se puede acceder
el servicio.

**NOTA:** Recuerda que la definición de la tarea se uso el mapeo del puerto 5000, por lo que debes
agregarlo al final del URL, por ejemplo: http://xx.xx.xx.xx:5000.

TODO: Pegar screenshot

## Limpieza

## Referencias

La siguiente es una lista de referencias que pueden usar para complementar
lo que se expone en esta sesión.

* [Guia](URL)
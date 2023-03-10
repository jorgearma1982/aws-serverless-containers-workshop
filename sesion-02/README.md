# Sesión 02: Serverless vs contenerización

## Introducción

En esta segunda sesión de work pondremos en práctica los diferentes arquitecturas para
entender mejor los servicios de tipo serverless, los contenedores y los principios de
los microservicios y desplegarlos usando los servicios de nube de AWS.

### Objetivos

Los objetivos especificos para el work de esta sesión son:

* Diferenciar entre los tipos de arquitectura
* Identificar sus ventajas y desventajas
* Analizar casos de uso de cada tipo de arquitectura

## Requisitos

Para poder realizar las siguientes actividades es necesario lo siguiente:

* Una computador con acceso a Internet
* Una cuenta de usuario de AWS
* Permisos de acceso para usar los servicios:
  * AWS Cloud9
  * AWS Cloudformation
  * AWS Lambda
  * AWS S3
  * AWS API Gateway
  * AWS DynamoDB
  * AWS Cloudwatch

Una vez que se ha creado el entorno cloud9, hacemos clic en `Open in Cloud9` para abrir el URl con
el entorno de desarrollo web.

En `Cloud9`, vamos a la `Terminal` y ahi hacemos las siguientes validaciones para ver que ya tengamos
docker configurado y listo para usarse.

## Actividades

En esta sesión realizaremos las siguientes actividades:

* Crearemos una Función como Servicio (FaaS) usando AWS Lambda
* Crear un bucket s3
* Exponer una API con API Gateway

### Crear función lambda con endpoint público

En esta actividad realizaremos un ejercicio donde crearemos una función HTTP como servicio usando AWS Lambda.

#### Ejecución

Para crear una función vamos al módulo de `Lambda` y hacemos clic en el botón `Crear una función`.
Usamos la opción de `Utilizar un proyecto` y luego en información básica, en `Select blueprint`
elegimos `Getting started with Lambda HTTP` basado en `nodejs14.x`, asignamos un nombre para la
función y definimos un nuevo rol para la función.

![Crear Lambda HTTP](images/aws-create-lambda-1.png)

Después de la información básica se muestra el código de la función y después nos informa acerca del
endpoint público que se va a crear y advierte sobre los controles de accesos.

![Crear Lambda HTTP](images/aws-create-lambda-2.png)

Y finalmente debemos hacer check en `Reconocimiento` para aceptar que la función se hará publica
y sobre la facturación, hacemos clic en `Crear una función` para que se inicie el aprovisionamiento.

#### Validación

Después de haber creado la función podremos ver la información generl de la función incluyendo el
`URL de la función`.

![Información función Lambda HTTP](images/aws-create-lambda-3.png)

Abrimos en nuestro navegador el URL para verificar que el servicio esta accesible.

![Prueba función Lambda HTTP](images/aws-test-lambda-1.png)

### Crear bucket de S3

En esta actividad realizaremos un ejercicio donde crearemos un bucket de almacenamiento de objetos
usando el servicio S3 de AWS.

#### Ejecución

Para crear un bucket vamos al módulo de `S3` y hacemos clic en el botón `Crear bucket`. En la
configuración general debemos elegir un nombre único y se elige la región en donde se desea
hospedar el bucket.

![Creación bucket S3](images/aws-create-s3-1.png)

Después en las opciones de propiedad de objetos, se debe usar la opción recomendada de `ACL deshabilitadas`.

![Creación bucket S3](images/aws-create-s3-2.png)

En la `configuración de bloque de acceso público para este bucket`, debemos quitar el check de la opción
`Bloquear todo el acceso público. Se debe hacer check en la opción de `Reconozco que la configuración
actual puede provocar que este bucket y los objetos que contiene se vuelvan públicos`.

![Creación bucket S3](images/aws-create-s3-3.png)

En las opciones de `control de versiones de buckets` por ahora lo dejamos en `Desactivar`.

![Creación bucket S3](images/aws-create-s3-4.png)

Y finalmente en las opciones de `cifrado predeterminado` dejamos en `tipo de clave de cifrado` la
opción de `Claves administradas por Amazon S3 (SSE-S3)`, y en `Clave de bucket` lo dejamos en
`Habilitar`.

![Creación bucket S3](images/aws-create-s3-5.png)

Para finalizar hacemos clic en `Crear bucket`.

Después de haber creado el bucket, vamos a subir el archivo de `logo.png` que descargamos en las
actividades de prework, hacemos clic en el nombre del bucket, y luego en la pestaña de `Objetos`
hacemos clic en el botón `Cargar` y en `Agregar archivos` buscamos el archivo en nuestra
máquina.

![Cargar archivo en bucket S3](images/aws-upload-s3-1.png)

Al final nos confirma de la carga y hacemos clic en `Cerrar`:

![Cargar archivo en bucket S3](images/aws-upload-s3-2.png)

Ahora, hacemos clic en el archivo `logo.png` y luego hacemos clic en `Acciones de objetos`
y selecionamos la opción de `Compartir con una URL prefirmada`.

![Compartir archivo en bucket S3](images/aws-share-s3-1.png)

#### Validación

El proceso de arriba nos dará un URL el cual podemos acceder desde el botón `Abrir`.

![Ver archivo en bucket S3](images/aws-view-s3-1.png)

### Exponer un servicio API

En esta actividad realizaremos un ejercicio donde usaremos `AWS CloudFormation` para aprovisionar
los diferentes servicios serverless para una API REST que usará funciones Lambda, un API Gateway
y una base de datos noSQL con DynamoDB.

Implementaremos la arquitectura que se describe en el siguiente diagrama:

![Arquitectura Serverless](images/aws-arch-stack-1.png)


#### Ejecución

En esta ocasión vamos a usar el servicio de `CloudFormation` para aprovisionar de forma automática
los servicios cloud que requerimos para exponer una API. La definición de los servicios que vamos
a implementar se definen en un archivo `YAML`.

Vamos al módulo `CloudFormation` y hacemos clic en `Crear nueva pila`, elegimos la opción de proyecto
existente y en la parte de especificar plantilla, debemos elegir la opción de `cargar un archivo de plantilla`
y luego en `elegir archivo` para seleccionarlo desde un directorio en la máquina local.

![Crear pila Cloudformation](images/aws-create-stack-1.png)

Hacemos clic en `Siguiente` y entonces damos nombre a la pila:

![Crear pila Cloudformation](images/aws-create-stack-2.png)

Hacemos clic en `Siguiente` y las demás opciones las dejamos con los valores predefinidos por ahora.

Al final en `Capacidades` debemos hacer check en `Confirmo que AWS CloudFormation podría crear recursos de IAM`,
y finalmente hacemos clic en `Enviar`, y podremos ver como los procesos inician con el aprovisionamiento.

![Crear pila Cloudformation](images/aws-create-stack-4.png)

Y al final de que se han aprovisionado los recursos vemos completado:

![Crear pila Cloudformation](images/aws-create-stack-5.png)


#### Validación

Una vez que hemos verificado que el proceso de aprovisionamiento de los recursos con `CloudFormation` terminó exitosamente, debemos validar la configuración de cada uno de los servicio.

##### Lambdas

Empezamos validando la función Lambda:

![Listar Lambdas](images/aws-list-lambdas-1.png)

Hagamos clic sobre el nombre de la función para ver sus detalles:

![Detalles Lambdas](images/aws-list-lambdas-2.png)

En la parte de abajo, en la pestaña `Código fuente` veremos el código actual que se implementó con la lambda:

![Código Lambdas](images/aws-code-lambdas-1.png)

Ahora, debemos eliminar el código y existente y remplazarlo con el contenido del archivo [index.js](code/index.js).

![Código Lambdas](images/aws-code-lambdas-2.png)

Después, veremos que aparece un mensaje que dice `Changes not deployed`, para aplicarlos hacemos clic en `Deploy`.

![Código Lambdas](images/aws-code-lambdas-3.png)

El resultado es el código actualizado:

![Código Lambdas](images/aws-code-lambdas-4.png)

Más adelante continuaremos con la lambda, por ahora hasta aquí lo dejamos.

##### DynamoDB

Ahora continuamos validando la base de datos DynamoDB donde registraremos las direcciones de correo:

![Listar DynamoDB](images/aws-list-dynamodb-1.png)

Podemos ver la información general, y hacer clic en `Explore los elementos de la tabla` para ver el
contenido.

![Detalles DynamoDB](images/aws-list-dynamodb-2.png)

##### API Gateway

Continuemos validando el servicio de API Gateway con el que expondremos nuestra función Lambda.

![Listar API Gateway](images/aws-list-api-gateway-1.png)

Vamos a `Recursos` y ahi seleccionamos el endpoint de `POST`:

![API Gateway POST](images/aws-api-gateway-post-1.png)

Hacemos clic en `PRUEBAS` y luego introducimos los valores de prueba, en `{userId}` ponemos `11` y en `cuerpo de solicitud` ponemos:

```json
{
    "email":"usuario11@api.com"
}
```

![API Gateway POST](images/aws-api-gateway-post-2.png)

Al final hacemos clic en `probar` y se debe hacer el registro en la tabla dynamodb.

Debería tener un resultado como este:

![API Gateway POST](images/aws-api-gateway-post-3.png)

Y si exploramos el contenido de la tabla veremos algo así:

![DynamoDB explorar tabla](images/aws-dynamodb-table-1.png)

Ahora vamos a ver como implementar la API, vamos al menú acciones y seleccionamos:

![API Gateway Deploy](images/aws-api-gateway-deploy-1.png)

Ahora nos aparece el cuadro de dialogo y lo llenamos así:

![API Gateway Deploy](images/aws-api-gateway-deploy-2.png)

Después se mostrará la siguiente resumen de configuración:

![API Gateway Deploy](images/aws-api-gateway-deploy-3.png)

Ahora que ya tenemos el endpoint, hacemos un cambio en el URL, pasando el parámetro `/users/11/hello`

![API Gateway Test](images/aws-api-gateway-test-1.png)

Finalmente vamos a validar el funcionamienot del dashboar de monitoreo del API Gateway, vamos al módulo `CloudWatch`, luego a `Paneles` y seleccionamos la opción
de `Paneles automatizados`.

![CloudWatch Panel](images/aws-cloudwatch-dashboard-1.png)

Seleccionamos el panel de `API Gateway` y vemos los detalles de las métricas que
se exportan:

![CloudWatch Panel](images/aws-cloudwatch-dashboard-1.png)

Dentro de este panel, pueden observar los siguientes datos importantes:

* **Count: Sum**: se refiere al número de invocaciones en el periodo definido
* **5XXError: Sum**: se refiere a los errores de lambda respondidos por el api, en mi caso es 0 y espero que en el tuyo también.
* **4XXError: Sum**: se refiere a todas las respuestas que regresan una ruta no encontrada, en este caso también es 0
* **Latency: Average**:  velocidad de primer respuesta entre cliente y servidor(promedio)
* **IntegrationLatency: Average**: velocidad de respuesta entre el API y Lambda

## Limpieza

Es sumamente importante señalar que después de que hemos realizado la actividad de la sesión
y hemos terminado de usar los recursos cloud que creamos es nuestra responsabilidad hacer
una limpieza de los recursos utilizados para no incurrir en más gastos en la cuenta AWS, por
lo que recomendados hacer una limpieza de los recursos.

## Referencias

La siguiente es una lista de referencias que pueden usar para complementar
lo que se expone en esta sesión.

* [Sin servidor en AWS](https://aws.amazon.com/es/serverless/?nc1=h_ls)
* [AWS Lambda](https://aws.amazon.com/es/lambda/)
* [Carácteristicas de AWS Lambda](https://aws.amazon.com/es/lambda/features/?pg=ln&sec=hs)
* [AWS S3](https://aws.amazon.com/es/s3/)
* [Carácteristicas de AWS S3](https://aws.amazon.com/es/s3/features/)
* [AWS CloudFormation](https://aws.amazon.com/es/cloudformation/)

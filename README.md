# Creación de aplicación web sin servidor con herramientas AWS: Lambda, API Gateway, Amplify, DynamoDB y Cognito.

Se implementará una aplicación web sencilla sin servidor que permite a los usuarios solicitar paseos en unicornio de la flota de Wild Rydes. La aplicación otorgará a los usuarios una interfaz de usuario basada en HTML para indicar la ubicación en la que desean que se los recoja y, también, interactuará con un servicio web RESTful en el backend para enviar la solicitud y un unicornio cercano. La aplicación también permitirá a los usuarios registrarse en el servicio e iniciar sesión antes de solicitar paseos.

**Acá se muestra la arquitectura de aplicaciones.**

<img width="633" alt="Captura de pantalla 2024-11-27 a la(s) 1 03 27 p m" src="https://github.com/user-attachments/assets/51288874-335a-4dc8-936a-cb627a0f20eb">

 * **Alojamiento web estático:** AWS Amplify aloja recursos web estáticos como HTML, CSS, JavaScript entre otros.
 * **Administración de usuarios:** Amazon Cognito proporciona funciones de administración y autenticación de usuarios.
 * **Backend sin servidor:** Amazon DynamoDB proporciona una capa de persistencia donde los datos se pueden almacenar mediante la función Lambda de la API.
 * **API RESTful:** JavaScript ejecutado en el navegador envía y recibe datos de una API de backend pública.

## 1: Alojamiento web estático

<img width="636" alt="Captura de pantalla 2024-11-27 a la(s) 2 25 25 p m" src="https://github.com/user-attachments/assets/59dbc66b-26a2-40e5-a748-a39fb511be00">

En espe punto se ha optado por utilizar el presente repositorio Github para manejo del código fuente y utilizaremos la consula `AWS Amplify` para implementar el sitio web. Amplify se encarga de configurar un lugar para almacenar el código de la aplicación web estática. Todo el contenido web estático, incluidos los archivos HTML, CSS, JavaScript y de imagen, entre otros, serán administrados por la consola de AWS Amplify. Después, los usuarios finales obtendrán acceso al sitio mediante la URL de sitio web público que se expone en la consola de AWS Amplify.

<img width="1172" alt="Captura de pantalla 2024-11-27 a la(s) 2 42 32 p m" src="https://github.com/user-attachments/assets/e3ec24b8-6e8c-4e67-8f92-a6e5b7d2fef0">

En la imagen adjunta se puede evidenciar ya implementada la aplicación web estatica en AWS Amplify y nos indica el dominio público al cuál podemos acceder a la misma.

<img width="1377" alt="Captura de pantalla 2024-11-27 a la(s) 2 44 02 p m" src="https://github.com/user-attachments/assets/aaf4102a-f248-49ec-8140-040aa8028a91">


**Nota:** Si se realiza cualquier modificación al sitio en el código fuente, este deberá ser actualizado y replicado en el repositorio Github para que este a su vez sea implementado en la consola de AWS Amplify.

## 2: Administración de usuarios

<img width="545" alt="Captura de pantalla 2024-11-27 a la(s) 2 25 57 p m" src="https://github.com/user-attachments/assets/992eba50-8b2d-45b4-8b09-780dc37dcfeb">

Para la administración de usuarios se utiliza `Amazon Cognito` para administrar las cuentas de los usuarios, se immplementará páginas que permiten a los usuarios registrarse como nuevo usuarios, verificar su dirección de correo electrónico e iniciar sesión en el sitio.

Una vez que los usuarios envíen su registro, Amazon Cognito enviará un correo electrónico de confirmación con un código de verificación a la dirección que hayan proporcionado. Para confirmar la cuenta, los usuarios deberán volver al sitio y especificarán la dirección de correo electrónico y el código de verificación que han recibido.

<img width="452" alt="Captura de pantalla 2024-11-27 a la(s) 2 29 11 p m" src="https://github.com/user-attachments/assets/29747821-efba-4ce1-a296-7d48ce54005c">

Una vez confirmada la cuenta en la consola de Amazon Cognito podemos validar los usuarios `Confirmados` y `Habilitados`.

<img width="1181" alt="Captura de pantalla 2024-11-27 a la(s) 2 30 05 p m" src="https://github.com/user-attachments/assets/4eb935fb-d329-49d1-91f1-79090410db54">

Una vez que los usuarios tengan una cuenta confirmada podrán iniciar sesión. Al iniciar sesión, los usuarios especifican su nombre de usuario (o correo electrónico) y contraseña. A continuación, una función JavaScript se comunica con Amazon Cognito, se autentica con el protocolo `Secure Remote Password (SRP)` y se le devuelve un conjunto de tokens web `JSON (JWT)`. Los tokens `JWT` contienen notificaciones sobre la identidad del usuario.

## 3: Backend de servicio sin servidor

En esta sección se utilizará `AWS Lambda` y `Amazon DynamoDB` para crear un proceso de backend destinado a administrar las solicitudes de la aplicación web. La aplicación antes implementada permite a los usuarios solicitar el envío de un unicornio a una ubicación de su elección. Para responder a esas solicitudes, el código JavaScript que se ejecuta en el navegador deberá invocar un servicio que se ejecuta en la nube.

<img width="635" alt="Captura de pantalla 2024-11-27 a la(s) 2 40 38 p m" src="https://github.com/user-attachments/assets/1c887026-481b-4d8f-97b2-d221aa96ffbb">

Se implementará una función Lambda que se invocará cada vez que un usuario solicite un unicornio. La función seleccionará un unicornio de la flota, registrará la solicitud en una tabla de DynamoDB y, después, responderá a la aplicación de frontend con detalles acerca del unicornio que se envía.

## 4: Implementar una API RESTful

En esta sección se utilizará `Amazon API Gateway` para exponer la función de Lambda creada en el paso anterior. La API estará disponible en la red de Internet pública. Se protegerá mediante el grupo de usuarios de Amazon Cognito que se realizón en el paso 2.

Con esta configuración se convierte el sitio web alojado estáticamente en una aplicación web dinámica al agregar código JavaScript en el lado del cliente que realiza llamadas AJAX a las API expuestas.

<img width="643" alt="Captura de pantalla 2024-11-27 a la(s) 2 52 07 p m" src="https://github.com/user-attachments/assets/6a7b6a60-e8ea-40af-823c-2d13c78c5c29">

En el diagram anterior se muestra como la API Gateway se integra con los componentes existentes que se creó en los pasos antes descritos. Una vez que los usuarios registrados se autentiquen podrán seleccionar su ubicación de recogida haciendo clic en un punto del mapa y, a continuación, solicitando un paseo mediante el botón "Request Unicorn".

<img width="1068" alt="Captura de pantalla 2024-11-27 a la(s) 2 59 18 p m" src="https://github.com/user-attachments/assets/b1b6bde7-e1a9-497e-8b69-6fd0e715d7b6">

En la imagen anterior nos muestra un mensaje que hemos sido auntenticados y nos muestra un `Token de autenticación`, dicho token es devuelto por la función JavaScript que se comunica con Amazon Cognito y devuelve un conjunto de tokens web `JSON (JWT)` que contiene notificaciones sobre la autenticidad del usuario. Paso realizado en el punto 2.

Una vez que seleccionamos un punto de recogida en el mapa le damos click en el botón `Request Unicorn`.

<img width="988" alt="Captura de pantalla 2024-11-27 a la(s) 3 04 34 p m" src="https://github.com/user-attachments/assets/052bdc50-5c5b-4d46-b6ca-c7e17d4b78f3">

Posterior a ello se debe ver una notificación en la barra lateral derecha de que un unicornio está en camino y, a continuación, ver un ícono de un unicornio que vuela hacia la ubicación de recogida. Posterior a su llegada al punto habrá una notificación de que el conductor ha llegado `Rocinante has arrived. Giddy up!`.

![taller-ezgif com-video-to-gif-converter](https://github.com/user-attachments/assets/0e5fc2fc-ad01-47b3-ba2f-cf7edc1d6e81)

Gracias.

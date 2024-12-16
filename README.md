# Creación de aplicación web sin servidor con herramientas AWS: Lambda, API Gateway, Amplify, DynamoDB y Cognito.

Se implementará una aplicación web sencilla sin servidor que permite a los usuarios solicitar paseos en unicornio de la flota de Wild Rydes. La aplicación otorgará a los usuarios una interfaz de usuario basada en HTML para indicar la ubicación en la que desean que se los recoja y, también, interactuará con un servicio web RESTful en el backend para enviar la solicitud y un unicornio cercano. La aplicación también permitirá a los usuarios registrarse en el servicio e iniciar sesión antes de solicitar paseos.

**Acá se muestra la arquitectura de aplicaciones.**

<img width="631" alt="Captura de pantalla 2024-12-16 a la(s) 9 14 34" src="https://github.com/user-attachments/assets/f24af07b-c970-448f-ba59-baa2de4221e0" />

 * **Alojamiento web estático:** AWS Amplify aloja recursos web estáticos como HTML, CSS, JavaScript entre otros.
 * **Administración de usuarios:** Amazon Cognito proporciona funciones de administración y autenticación de usuarios.
 * **Backend sin servidor:** Amazon DynamoDB proporciona una capa de persistencia donde los datos se pueden almacenar mediante la función Lambda de la API.
 * **API RESTful:** JavaScript ejecutado en el navegador envía y recibe datos de una API de backend pública.

## 1: Alojamiento web estático

<img width="642" alt="Captura de pantalla 2024-12-16 a la(s) 9 16 57" src="https://github.com/user-attachments/assets/102657c0-85ea-49b2-bc7a-b9bc8e90cdcb" />

En espe punto se ha optado por utilizar el presente repositorio Github para manejo del código fuente y utilizaremos la consula `AWS Amplify` para implementar el sitio web. Amplify se encarga de configurar un lugar para almacenar el código de la aplicación web estática. Todo el contenido web estático, incluidos los archivos HTML, CSS, JavaScript y de imagen, entre otros, serán administrados por la consola de AWS Amplify. Después, los usuarios finales obtendrán acceso al sitio mediante la URL de sitio web público que se expone en la consola de AWS Amplify.

<img width="1013" alt="Captura de pantalla 2024-12-16 a la(s) 9 17 28" src="https://github.com/user-attachments/assets/a3cc6b16-0ff9-424e-aabc-4833b7c1ce9f" />

En la imagen adjunta se puede evidenciar ya implementada la aplicación web estatica en AWS Amplify y nos indica el dominio público al cuál podemos acceder a la misma.

<img width="1013" alt="Captura de pantalla 2024-12-16 a la(s) 9 17 46" src="https://github.com/user-attachments/assets/80164d08-2c11-431b-91ad-cdcadccfa8cd" />


**Nota:** Si se realiza cualquier modificación al sitio en el código fuente, este deberá ser actualizado y replicado en el repositorio Github para que este a su vez sea implementado en la consola de AWS Amplify.

## 2: Administración de usuarios

<img width="522" alt="Captura de pantalla 2024-12-16 a la(s) 9 25 10" src="https://github.com/user-attachments/assets/5c37e484-1f52-4b3b-93ad-a7b50ba2d2c5" />

Para la administración de usuarios se utiliza `Amazon Cognito` para administrar las cuentas de los usuarios, se immplementará páginas que permiten a los usuarios registrarse como nuevo usuarios, verificar su dirección de correo electrónico e iniciar sesión en el sitio.

Una vez que los usuarios envíen su registro, Amazon Cognito enviará un correo electrónico de confirmación con un código de verificación a la dirección que hayan proporcionado. Para confirmar la cuenta, los usuarios deberán volver al sitio y especificarán la dirección de correo electrónico y el código de verificación que han recibido.

<img width="428" alt="Captura de pantalla 2024-12-16 a la(s) 9 18 06" src="https://github.com/user-attachments/assets/0aead749-0b8c-4d1f-af53-cff73360ba79" />

Una vez confirmada la cuenta en la consola de Amazon Cognito podemos validar los usuarios `Confirmados` y `Habilitados`.

<img width="1026" alt="Captura de pantalla 2024-12-16 a la(s) 9 18 22" src="https://github.com/user-attachments/assets/c4b2b7c7-c4ad-44de-8036-f28e36c22c2a" />

Una vez que los usuarios tengan una cuenta confirmada podrán iniciar sesión. Al iniciar sesión, los usuarios especifican su nombre de usuario (o correo electrónico) y contraseña. A continuación, una función JavaScript se comunica con Amazon Cognito, se autentica con el protocolo `Secure Remote Password (SRP)` y se le devuelve un conjunto de tokens web `JSON (JWT)`. Los tokens `JWT` contienen notificaciones sobre la identidad del usuario.

## 3: Backend de servicio sin servidor

En esta sección se utilizará `AWS Lambda` y `Amazon DynamoDB` para crear un proceso de backend destinado a administrar las solicitudes de la aplicación web. La aplicación antes implementada permite a los usuarios solicitar el envío de un unicornio a una ubicación de su elección. Para responder a esas solicitudes, el código JavaScript que se ejecuta en el navegador deberá invocar un servicio que se ejecuta en la nube.

<img width="635" alt="Captura de pantalla 2024-12-16 a la(s) 9 25 48" src="https://github.com/user-attachments/assets/dab9e6a0-1688-4c49-9b3c-a6e737ebce77" />

Se implementará una función Lambda que se invocará cada vez que un usuario solicite un unicornio. La función seleccionará un unicornio de la flota, registrará la solicitud en una tabla de DynamoDB y, después, responderá a la aplicación de frontend con detalles acerca del unicornio que se envía.

## 4: Implementar una API RESTful

En esta sección se utilizará `Amazon API Gateway` para exponer la función de Lambda creada en el paso anterior. La API estará disponible en la red de Internet pública. Se protegerá mediante el grupo de usuarios de Amazon Cognito que se realizón en el paso 2.

Con esta configuración se convierte el sitio web alojado estáticamente en una aplicación web dinámica al agregar código JavaScript en el lado del cliente que realiza llamadas AJAX a las API expuestas.

<img width="637" alt="Captura de pantalla 2024-12-16 a la(s) 9 26 15" src="https://github.com/user-attachments/assets/b7f09220-2810-4310-bb1e-12daef725211" />

En el diagram anterior se muestra como la API Gateway se integra con los componentes existentes que se creó en los pasos antes descritos. Una vez que los usuarios registrados se autentiquen podrán seleccionar su ubicación de recogida haciendo clic en un punto del mapa y, a continuación, solicitando un paseo mediante el botón "Request Unicorn".

<img width="1014" alt="Captura de pantalla 2024-12-16 a la(s) 9 18 37" src="https://github.com/user-attachments/assets/f0266127-d84d-4165-9635-f7ec3a2520dd" />

En la imagen anterior nos muestra un mensaje que hemos sido auntenticados y nos muestra un `Token de autenticación`, dicho token es devuelto por la función JavaScript que se comunica con Amazon Cognito y devuelve un conjunto de tokens web `JSON (JWT)` que contiene notificaciones sobre la autenticidad del usuario. Paso realizado en el punto 2.

Una vez que seleccionamos un punto de recogida en el mapa le damos click en el botón `Request Unicorn`.

<img width="991" alt="Captura de pantalla 2024-12-16 a la(s) 9 18 51" src="https://github.com/user-attachments/assets/3ed9d6c1-eeef-4920-9229-c9a03ccbdcdf" />

Posterior a ello se debe ver una notificación en la barra lateral derecha de que un unicornio está en camino y, a continuación, ver un ícono de un unicornio que vuela hacia la ubicación de recogida. Posterior a su llegada al punto habrá una notificación de que el conductor ha llegado `Rocinante has arrived. Giddy up!`.

![390555929-0e5fc2fc-ad01-47b3-ba2f-cf7edc1d6e81](https://github.com/user-attachments/assets/6219c1fa-1432-4898-9617-1f97801f4a37)

Gracias.

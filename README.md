# PPS-Unidad3Actividad9-RFI
Explotación y Mitigación de Remote File Inclusion (RFI)

Tenemos como objetivo:

> - Ver cómo se pueden hacer ataques Inclusión de Archivos Remotosn (RFI).
>
> - Analizar el código de la aplicación que permite ataques de Inclusión de Archivos Remotosn (RFI).
>
> - Implementar diferentes modificaciones del codigo para aplicar mitigaciones o soluciones.

## ¿Qué es CSRF?
---

La vulnerabilidad de Inclusión de archivos permite a un atacante incluir un archivo, generalmente explotando un mecanismo “dynamic file inclusion” implementado en la aplicación de destino. La vulnerabilidad se produce debido al uso de la entrada suministrada por el usuario sin la validación adecuada.

Esto puede conducir a algo como la salida del contenido del archivo, pero dependiendo de la gravedad, también puede conducir a:

- Ejecución de código en el servidor web

- Ejecución de código en el lado del cliente, como JavaScript, que puede conducir a otros ataques, como secuencias de comandos en sitios cruzados (XSS)

- Denegación de Servicio (DoS)

- Divulgación de Información Sensible

Remote File Inclusion (también conocido como RFI) es el proceso de incluir archivos remotos a través de la explotación de procedimientos de inclusión vulnerables implementados en la aplicación. Esta vulnerabilidad se produce, por ejemplo, cuando una página recibe, como entrada, la ruta al archivo que tiene que incluirse y esta entrada no se desinfecta correctamente, lo que permite inyectar una URL externa. Aunque la mayoría de los ejemplos apuntan a scripts PHP vulnerables, debemos tener en cuenta que también es común en otras tecnologías como JSP, ASP y otras.
 
## ACTIVIDADES A REALIZAR
---
> Lee detenidamente la sección de vulnerabilidades de subida de archivos.  de la página de PortWigger <https://portswigger.net/web-security/file-upload>
>
> Lee el siguiente [documento sobre Explotación y Mitigación de ataques de Remote Code Execution](./files/ExplotacionYMitigacionRFI.pdf>
> 
> También y como marco de referencia, tienes [ la sección de correspondiente de ataque de inclusión de archivos remotos de la **Proyecto Web Security Testing Guide** (WSTG) del proyecto **OWASP**.]<https://owasp.org/www-project-web-security-testing-guide/v42/4-Web_Application_Security_Testing/07-Input_Validation_Testing/11.2-Testing_for_Remote_File_Inclusion>
>

### Configuración para deshabilitar la seguridad en PHP 8.2 (sólo para pruebas)

Para poder realizar la actividad vamos a deshabilitar la seguridad y así poder trabajar la vulnerabilidad correctamente. Lo realizaremos cambiando la configuración de PHP.

Para ello nos conectamos a nuestro contenedor si estás utilizando es escenario con docker:

~~~
docker exec -it lamp-php83 /bin/bash
~~~

 y una vez que nos hemos conectado, guardamos una copia de seguridad del archivo de configuración para volverlo a restaurar al final de la actividad:

~~~
cd /usr/local/etc/php/
cp php.ini php.ini-original
nano php.ini
~~~

Añadimos al final las variables indicadas:

~~~
disable_functions =
allow_url_include = On
allow_url_fopen = On
open_basedir = 
~~~

Una vez cambiada la configuración, reiniciamos el servicio o en el caso de que utilicemos docker, reiniciamos el contenedor:

~~~
docker-compose restart webserver
~~~

![](images/rfi1.png)


Aquí puedes encontrar el fichero de configuración [php.ini](files/php.ini.rfi).



Ejecutar el siguiente comando en la terminal como root para modificar php.ini y así deshabilitar todas las restricciones:
sudo sed -i 's/^disable_functions.*/disable_functions =/' /etc/php/8.2/apache2/php.ini && \
sudo sed -i 's/^allow_url_include.*/allow_url_include = On/' /etc/php/8.2/apache2/php.ini &&
\
sudo sed -i 's/^allow_url_fopen.*/allow_url_fopen = On/' /etc/php/8.2/apache2/php.ini && \
sudo sed -i 's/^open_basedir.*/;open_basedir =/' /etc/php/8.2/apache2/php.ini && \
sudo systemctl restart apache2

¿Qué hacemos con estas configuraciones?

1. Elimina todas las funciones deshabilitadas (disable_functions vacío).

2. Habilita la inclusión de archivos remotos (allow_url_include = On).

3. Habilita file_get_contents() para URLs externas (allow_url_fopen = On).

4. Desactiva open_basedir para permitir la ejecución en cualquier directorio.

5. Reinicia Apache para aplicar los cambios.
## Código vulnerable
---



![](images/.png)

## Mitigación de RFI
---

La solución más efectiva para eliminar las vulnerabilidades de inclusión de archivos es evitar pasar la entrada enviada por el usuario a cualquier API de sistema de archivos/marco. Si esto no es posible, la aplicación puede mantener una lista de permisos de archivos que puede incluir la página y, a continuación, utilizar un identificador (por ejemplo, el número de índice) para acceder al archivo seleccionado. Cualquier solicitud que contenga un identificador no válido debe rechazarse para que no haya oportunidad de que los usuarios maliciosos manipulen la ruta. Mira el [Archivo de Hoja de Trucos para buenas prácticas de seguridad](https://cheatsheetseries.owasp.org/cheatsheets/File_Upload_Cheat_Sheet.html) en este tema.

Vamos realizando operaciones.


### **Código seguro**
---

Aquí está el código securizado:

🔒 Medidas de seguridad implementadas

- :

        - 

        - 



🚀 Resultado

✔ 

✔ 

✔ 

### Dejando todo en orden
----

Recuerda volver a poner el archivo php.ini original:

~~~
cd /usr/local/etc/php/
cp php.ini-original php.ini
~~~

Una vez cambiada la configuración, reiniciamos el servicio o en el caso de que utilicemos docker, reiniciamos el contenedor:

~~~
docker-compose restart webserver
~~~


## ENTREGA

> __Realiza las operaciones indicadas__

> __Crea un repositorio  con nombre PPS-Unidad3Actividad6-Tu-Nombre donde documentes la realización de ellos.__

> No te olvides de documentarlo convenientemente con explicaciones, capturas de pantalla, etc.

> __Sube a la plataforma, tanto el repositorio comprimido como la dirección https a tu repositorio de Github.__


# Máquina: Tech_Supp0rt-1

**Tryhackme: Tech_Supp0rt-1**

Lo primero que haremos, será lanzar un NMAP para ver qué puertos tiene abiertos la máquina:

![TCHSUPP1](https://github.com/AntonioPC94/Tech_Supp0rt-1/blob/1d634cc0db32dfb86052946b2c31ae495d962246/img/TCHSUPP1.png)

![TCHSUPP2](https://github.com/AntonioPC94/Tech_Supp0rt-1/blob/1d634cc0db32dfb86052946b2c31ae495d962246/img/TCHSUPP2.png)

Como se observa en las imágenes anteriores, existen 4 servicios que nos pueden interesar:

- Puerto 22 (SSH)
- Puerto 80 (HTTP)
- Puerto 139 (NetBIOS-ssn)
- Puerto 445 (SMB)

A continuación, usaremos el módulo de escaneo de vulnerabilidades de NMAP para ver si este nos devuelve algo interesante sobre los puertos anteriormente encontrados:

![TCHSUPP3](https://github.com/AntonioPC94/Tech_Supp0rt-1/blob/1d634cc0db32dfb86052946b2c31ae495d962246/img/TCHSUPP3.png)

En el "http-enum", veremos que existen 3 directorios que pueden ser de interés:

- /test/
- /phpinfo.php
- /wordpress/wp.login.php

Los analizamos y después de haber accedido a cada uno de ellos, vemos que en el tercero hay un login de WordPress.

![TCHSUPP4](https://github.com/AntonioPC94/Tech_Supp0rt-1/blob/1d634cc0db32dfb86052946b2c31ae495d962246/img/TCHSUPP4.png)

Ahora, si accedemos al directorio "/wordpress/", veremos que se nos abre la siguiente página:

![TCHSUPP5](https://github.com/AntonioPC94/Tech_Supp0rt-1/blob/1d634cc0db32dfb86052946b2c31ae495d962246/img/TCHSUPP5.png)

Analizando la página con Wappalyzer, veremos la siguiente información sobre ella:

![TCHSUPP6](https://github.com/AntonioPC94/Tech_Supp0rt-1/blob/1d634cc0db32dfb86052946b2c31ae495d962246/img/TCHSUPP6.png)

Si observamos el contenido de la página, veremos que en el apartado "BLOG", un usuario ha realizado una publicación llamada "Hello World". Bien, ahora vamos a utilizar la herramienta "WP-Scan" para sacar toda la información que podamos de dicha página web y ver si existen, entre otras cosas, más usuarios aparte del que ya hemos encontrado:

- Listamos los usuarios:

![TCHSUPP7](https://github.com/AntonioPC94/Tech_Supp0rt-1/blob/1d634cc0db32dfb86052946b2c31ae495d962246/img/TCHSUPP7.png)

Como se observa en la imagen anterior, solo existe un usuario registrado en la BDD de la página web. La orden utilizada muestra más resultados, pero ninguno es relevante a simple vista.

Bien, ahora vamos a irnos con el servicio SMB que está en el puerto 445 de la máquina objetivo. Lo primero que haremos, será listar los recursos compartidos que tiene el servidor SMB, a ver si encontramos algo que nos pueda interesar.

![TCHSUPP8](https://github.com/AntonioPC94/Tech_Supp0rt-1/blob/55e9009fd6049e4bab18a289504ca8550cb14897/img/TCHSUPP8.png)

Como vemos en la imagen anterior, existen 2 directorios dentro del servidor que posiblemente contengan información relevante.

A continuación, vamos a intentar acceder como usuario "Anónimo" a la segunda carpeta mostrada en la imagen anterior lanzando el siguiente comando:

![TCHSUPP9](https://github.com/AntonioPC94/Tech_Supp0rt-1/blob/55e9009fd6049e4bab18a289504ca8550cb14897/img/TCHSUPP9.png)

Con un poco de lógica, podremos deducir que la contraseña para acceder como usuario "Anónimo", es a*********.

Una vez dentro, listamos el contenido del directorio "websvr".

![TCHSUPP10](https://github.com/AntonioPC94/Tech_Supp0rt-1/blob/55e9009fd6049e4bab18a289504ca8550cb14897/img/TCHSUPP10.png)

Como se observa en la imagen anterior, vemos que existe un fichero ".txt" que es posible que contenga información relevante para nosotros, así que nos lo vamos a descargar del servidor y vamos a mostrar su contenido:

![TCHSUPP11](https://github.com/AntonioPC94/Tech_Supp0rt-1/blob/55e9009fd6049e4bab18a289504ca8550cb14897/img/TCHSUPP11.png)

![TCHSUPP12](https://github.com/AntonioPC94/Tech_Supp0rt-1/blob/55e9009fd6049e4bab18a289504ca8550cb14897/img/TCHSUPP12.png)

En la imagen anterior se muestran unas credenciales de una página web llamada "Subrion", de la cual no sabemos nada todavía.

Bien, ahora vamos a coger el hash que hemos obtenido y vamos a intentar crackearlo para poder sacar la contraseña del usuario "admin" de dicha página.

Para crackear el hash obtenido, nos iremos a CyberChef y lo colocaremos en el apartado de "Input". A continuación, le daremos a "Bake" y automáticamente el programa buscará la manera de crackear el hash que le hemos proporcionado.

Una vez crackeado, le daremos a la varita mágica en el "Output" y veremos la contraseña del usuario "admin" en claro.

![TCHSUPP13](https://github.com/AntonioPC94/Tech_Supp0rt-1/blob/55e9009fd6049e4bab18a289504ca8550cb14897/img/TCHSUPP13.png)

Como no tenemos mucha idea de qué es Subrion, vamos a informarnos por internet sobre dicha página y vamos a ver si podemos acceder al panel de administración de la misma.

![TCHSUPP14](https://github.com/AntonioPC94/Tech_Supp0rt-1/blob/55e9009fd6049e4bab18a289504ca8550cb14897/img/TCHSUPP14.png)

Como vemos en la imagen anterior, Subrion es un CMS de código abierto. Haciéndole una consulta a ChatGPT, veremos que para acceder al panel de control de dicho CMS, tendremos que poner en la barra de direcciones: http://(DirecciónIP)/subrion/panel/.

![TCHSUPP15](https://github.com/AntonioPC94/Tech_Supp0rt-1/blob/55e9009fd6049e4bab18a289504ca8550cb14897/img/TCHSUPP15.png)

Bien, ahora colocaremos las credenciales que sacamos anteriornmente y accederemos al panel de control del CMS.

![TCHSUPP16](https://github.com/AntonioPC94/Tech_Supp0rt-1/blob/4991e55ceab65a6e956a62bde641d5ec1a9bc4e6/img/TCHSUPP16.png)

Observando la página web, veremos que tiene un apartado para poder subir archivos.

![TCHSUPP17](https://github.com/AntonioPC94/Tech_Supp0rt-1/blob/4991e55ceab65a6e956a62bde641d5ec1a9bc4e6/img/TCHSUPP17.png)

Entonces, lo que vamos a hacer es subir una reverse shell al servidor para poder ganar acceso a la máquina.

Pero antes, tendremos que mirar qué tipo de archivos permite subir el CMS. Para ello, usaremos la extensión Wappalyzer para ver qué información nos arroja sobre la página.

![TCHSUPP18](https://github.com/AntonioPC94/Tech_Supp0rt-1/blob/4991e55ceab65a6e956a62bde641d5ec1a9bc4e6/img/TCHSUPP18.png)

Como se observa en la imagen anterior, la plataforma soporta PHP.

Aparte de lo visto anteriormente, existe también un apartado de la plataforma llamado "PHP Info", el cual nos muestra qué extensiones de archivo soporta el sitio web.

![TCHSUPP19](https://github.com/AntonioPC94/Tech_Supp0rt-1/blob/4991e55ceab65a6e956a62bde641d5ec1a9bc4e6/img/TCHSUPP19.png)

Bien, sabiendo todo esto, vamos a irnos al repositorio de Github de PentestMonkey y vamos a cocinar nuestra "Reverse Shell" antes de subirla al servidor.

Nota: Al intentar subirla en ".php", devuelve un "Forbidden", entonces la vamos a subir en "phar".

Modificamos la IP y el puerto por el que vamos a escuchar para obtener la "Reverse Shell".

![TCHSUPP20](https://github.com/AntonioPC94/Tech_Supp0rt-1/blob/4991e55ceab65a6e956a62bde641d5ec1a9bc4e6/img/TCHSUPP20.png)

Guardamos la configuración y en este caso, le pondremos como nombre al fichero "Kinder.php".

Una vez guardado el archivo, le cambiaremos la extensión a ".phar".

Ahora ponemos a escuchar a nuestro equipo en el puerto configurado.

![TCHSUPP21](https://github.com/AntonioPC94/Tech_Supp0rt-1/blob/4991e55ceab65a6e956a62bde641d5ec1a9bc4e6/img/TCHSUPP21.png)

Subimos el archivo a la plataforma y lo llamamos desde la URL de la página.

![TCHSUPP22](https://github.com/AntonioPC94/Tech_Supp0rt-1/blob/4991e55ceab65a6e956a62bde641d5ec1a9bc4e6/img/TCHSUPP22.png)

En la siguiente imagen, veremos que una vez llamamos al archivo desde la URL, ganamos acceso a la máquina objetivo.

![TCHSUPP23](https://github.com/AntonioPC94/Tech_Supp0rt-1/blob/4991e55ceab65a6e956a62bde641d5ec1a9bc4e6/img/TCHSUPP23.png)

A continuación, upgradearemos la shell para que podamos trabajar más cómodos.

![TCHSUPP24]()

Bien, ahora que tenemos una shell totalmente interactiva, lo primero que vamos a hacer, es ver si podemos listar el contenido del fichero "/etc/passwd" para ver qué usuarios existen en el sistema.

![TCHSUPP25]()

Como se observa en la imagen anterior, aparte del usuario "root", existe otro usuario en el sistema objetivo.

Ahora que sabemos lo visto anteriormente, vamos a dar comienzo a la fase de escalada. Como vimos anteriormente, la máquina contenía una página de WordPress en la cual solo había realizado una publicación el usuario que mencionamos. Ahora bien, ese usuario tiene que estar registrado si o si en la BDD de datos de WordPress.

Entonces, ahora que tenemos acceso a los archivos internos de WordPress, vamos a listar el contenido del fichero "wp-config" a ver si encontramos más información sobre dicho usuario.

![TCHSUPP26]()

Como se observa en la imagen anterior, en el fichero de configuración de WordPress tenemos las credenciales en claro del usuario que encontramos al principio.

Tras revisar el panel de administración de WordPress y no haber encontrado información relevante, lo que vamos a hacer es probar la contraseña que encontramos anteriormente en el fichero de configuración de WordPress con el usuario que nos encontramos en el fichero "/etc/passwd".

Para ello, nos abriremos una nueva pestaña en la terminal y nos intentaremos conectar por SSH al usuario mencionado anteriormente.

![TCHSUPP27]()

Como se observa en la imagen anterior, ya estaríamos logueados en el sistema objetivo como el usuario que indicamos anteriormente.

Ahora lo que haremos será utilizar el comando "sudo -l" para ver qué comandos puede utilizar el usuario con privilegios elevados.

![TCHSUPP28]()

Como se observa en la imagen anterior, el usuario puede lanzar el binario "iconv" con privilegios elevados.

A continuación, nos iremos a la página GTFOBins y buscaremos el binario mencionado anteriormente en su buscador para ver si nos devuelve alguna manera de escalar privilegios en el sistema objetivo utilizando dicho binario.

![TCHSUPP29]()


La página nos muestra un comando que nos permitirá el archivo que le metamos en la pate de la variable "$LFILE".

Entonces lo que vamos a hacer, es intentar leer el fichero "root.txt" para que el sistema nos devuelva la flag que estamos buscando.

Para ello, utilizaremos el siguiente comando: sudo -u root iconv  -f 8859_1 -t 8859_1 "/root/root.txt"

![TCHSUPP30]()














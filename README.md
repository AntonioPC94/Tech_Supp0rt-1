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

Los analizamos y después de haber accedido a cada uno de ellos, vemos que en el tercero efectivamente aparece un login de WordPress.

![TCHSUPP4](https://github.com/AntonioPC94/Tech_Supp0rt-1/blob/1d634cc0db32dfb86052946b2c31ae495d962246/img/TCHSUPP4.png)

Ahora, si accedemos al directorio "/wordpress/", veremos que se nos abre la siguiente página:

![TCHSUPP5](https://github.com/AntonioPC94/Tech_Supp0rt-1/blob/1d634cc0db32dfb86052946b2c31ae495d962246/img/TCHSUPP5.png)

Analizando la página con Wappalyzer, veremos la siguiente información sobre ella:

![TCHSUPP6](https://github.com/AntonioPC94/Tech_Supp0rt-1/blob/1d634cc0db32dfb86052946b2c31ae495d962246/img/TCHSUPP6.png)

Si observamos el contenido de la página, veremos que en el apartado "BLOG", un usuario ha realizado una publicación llamada "Hello World". Bien, ahora vamos a utilizar la herramienta "WP-Scan" para sacar toda la información que podamos de dicha página web y ver si existen, entre otras cosas, más usuarios aparte del que ya hemos encontrado:

- Listamos los usuarios:

![TCHSUPP7](https://github.com/AntonioPC94/Tech_Supp0rt-1/blob/1d634cc0db32dfb86052946b2c31ae495d962246/img/TCHSUPP7.png)

Bien, ahora vamos a irnos con el servicio SMB que está en el puerto 445 de la máquina objetivo. Lo primero que haremos, será listar los recursos compartidos que tiene el servidor SMB, a ver si encontramos algo que nos pueda interesar.

![TCHSUPP8]

Como vemos en la imagen anterior, existen 2 directorios dentro del servidor que posiblemente contengan información relevante.

A continuación, vamos a intentar acceder como usuario "Anónimo" al servidor lanzando el siguiente comando:

![TCHSUPP9]

Con un poco de lógica, podremos deducir que la contraseña para acceder como usuario "Anónimo", es a*********.

Una vez dentro, listamos el contenido del directorio "websvr".

![TCHSUPP10]

Como se observa en la imagen anterior, vemos que existe un fichero ".txt" que es posible que contenga información relevante para nosotros, así que nos lo vamos a descargar del servidor y vamos a mostrar su contenido:

![TCHSUPP11]

![TCHSUPP12]

En la imagen anterior se muestran unas credenciales de una página web llamada "Subrion", de la cual no sabemos nada todavía.

Bien, ahora vamos a coger el hash que hemos obtenido y vamos a intentar crackearlo para poder sacar la contraseña del usuario "admin" de dicha página.

Para crackear el hash obtenido, nos iremos a CyberChef y lo colocaremos en el apartado de "Input". A continuación, le daremos a "Bake" y automáticamente el programa buscará la manera de crackear el hash que le hemos proporcionado.

Una vez crackeado, le daremos a la varita mágica en el "Output" y veremos la contraseña del usuario "admin" en claro.

![TCHSUPP13]

Como no tenemos mucha idea de qué es Subrion, vamos a informarnos por internet sobre dicha página y vamos a ver si podemos acceder al panel de administración de la misma.

![TCHSUPP14]

Como vemos en la imagen anterior, Subrion es un CMS de código abierto. Haciéndole una consulta a ChatGPT, veremos que para acceder al panel de control de dicho CMS, tendremos que poner en la barra de direcciones: http://(DirecciónIP)/subrion/panel/.

![TCHSUPP15]



























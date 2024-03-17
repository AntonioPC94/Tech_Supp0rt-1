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

A continuación, vamos a usar WPScan para hacer fuerza bruta a la página de WordPress y poder sacar así la contraseña de dicho usuario.
















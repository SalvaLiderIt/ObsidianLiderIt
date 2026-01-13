Si quiero conectarme desde mi casa con el portatil a la red interna de lider, necesito activar el vpn, utilizando el programa sophos 
![[Pasted image 20251023092331.png]]
El usuario es spavila para la contraseña tengo que poner mi misma contraseña de usuario del pc seguida del código de doble factor que está en el movil en authenticator 

Para descargar la configuración de VPN y sophos, en el navegador acceder a https://ssl.liderit.es:4445/ al igual introducir usuario y contraseña con el código de doble factor a continuación 
Hacer doble click en el archivo descargado de "Use with sophos connect and Open VPN Connect v2 clients"
![[Pasted image 20251023092704.png]]

En caso de que no se conecte la máquina virtual a internet (ethernet), he conseguido solucionarlo simplemente al cambiar la VPN y conectarme con sophos a la red de la oficina desde la máquina física. Después he reiniciado la máquina virtual y ya tenía acceso a internet.
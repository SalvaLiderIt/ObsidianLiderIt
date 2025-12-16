
- comando para buscar las extensiones publicada
- ```shell
  Get-NAVAppInfo -ServerInstance BC260 | Where-Object { $_.Version -eq '1.0.0.0' }
  ```

Este comando es para desinstalar una extensión desde shell

```shell
Uninstall-NAVApp -ServerInstance BC260 -Name "CursoTraducciones" -Tenant default
```

Este comando es para Despublicar una extensión desde shell

```shell
Unpublish-NAVApp -ServerInstance BC260 -Name "CursoTraducciones"
```

Para Solucionar problema de conexión a business central, cuando no se ha ejecutado el servicio, se puede comprobar y reiniciar desde Shell Business Central Administration
Para solucionar el error de conexión de puertos a la hora de publicar desde visual code, abrir Business Central Administration Shell --> comprobar si la instancia de navserver está "running" con el comando "get-navserverInstance -ServerInstance BC260" , si pone "start pending" metemos el siguiente comando "Restart-navserverInstance -ServerInstance BC260" sin las comillas. Véase en la imagen, esperamos unos 5 minutos y al reiniciarse volvemos a probar

```shell
get-navserverInstance -ServerInstance BC260
```

Y con este lo reiniciamos

```shell
Restart-navserverInstance -ServerInstance BC260
```

![[Pasted image 20251020125143.png]]
### ¿Qué es?
Propiedad que permite definir una vista filtrada de los registros que se mostrarán en una página, directamente desde la definición del objeto.

### Sintaxis
```
SourceTableView = where(<Campo> = const(<Valor>));
```

### Ejemplo práctico

```
SourceTableView = where(Blocked = const(true));
```

### Complemento útil

Para crear dos vistas separadas (activas vs archivadas), puedes definir dos páginas distintas con filtros opuestos:

- Página de activas:

  ```
   SourceTableView = where(Blocked = const(false));
   ```

- Página de archivadas:

  ```
   SourceTableView = where(Blocked = const(true));
    ```


Esto permite mantener una interfaz clara y segmentada según el estado del registro.

### Consideraciones

- Solo funciona en objetos `Page`.
- Se aplica al cargar la página, no requiere filtros adicionales en tiempo de ejecución.
- Ideal para páginas tipo `List` o `ListPart`.

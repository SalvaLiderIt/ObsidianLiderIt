
Ejemplo de procedimiento vacío y su estructura
```al
local procedure MoveNameToUpperCase(TheName : Text) : Text

    begin

        exit(UpperCase(TheName));

    end;

}
```

La estructura sería tal que así
- Modificador de acceso (local/internal/procedure) nombreProcedimiento(nombreParametro:TipodeDato) : TipoDeDatoqueDevuelve
- [Visibilidad] procedure NombreProcedimiento([Parámetros]): TipoRetorno
- 
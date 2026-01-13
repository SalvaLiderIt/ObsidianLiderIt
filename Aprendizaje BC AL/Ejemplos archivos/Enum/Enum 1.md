
```al
enum 50100 Nutriscore
{
    Extensible = false;

    value(0; " ")//es buena practica dejar un valor en blanco para que no haya ninguno seleccionado por defecto
    {
        Caption = ' ', Locked = true;

    }
    value(1; A)
    {
        Caption = 'A';
    }
    value(2; B)
    {
        Caption = 'B';
    }
    value(3; C)
    {
        Caption = 'C';
    }
    value(4; D)
    {
        Caption = 'D';
    }
    value(5; E)
    {
        Caption = 'E';
    }
}
```

Los `systemparts` son componentes especiales que permiten mostrar funcionalidades estándar como **Notas**, **Vínculos** y **Adjuntos** en el panel lateral derecho (FactBox) de una página. Al activarlos, el usuario puede ver y gestionar estos elementos directamente desde la interfaz.

- **Solo funciona en páginas que muestran un solo registro** (como `Card` o `Document`). En páginas tipo `ListPart`, no se mostrará el panel lateral automáticamente.
    
- **El panel se activa con el botón "i"** en la parte superior derecha de la página.
    
- **Los datos se guardan por registro**, lo que permite tener notas y adjuntos únicos por cada ficha.

Agrega esta sección dentro de tu definición de página AL, a la misma altura que content, como hermana, dentro de layout.

```al
area(FactBoxes)
        {
            systempart(Notes; Notes)
            {}
            systempart(Links; Links)
            {}
            systempart(MyNotes; MyNotes)
            {}
        }
```

Ejemplo completo de layout dentro de una page 
```al
layout

    {
        area(Content)
        {
            group(GroupName)
            {
                Caption = 'Vendor Certificates Header';
                field(CertifiedCode; Rec.CertifiedCode)
                {
                }
                field(CertifiedType; Rec.CertifiedType)
                {
                }
                field(IssueDate; Rec.IssueDate)
                {
                }
                field(ExpirationDate; Rec.ExpirationDate)
                {
                }
                field(Active; Rec.Active)
                {
                }
                /* field(Attached; Rec.Attached)
                {
                } */
            }

            part(VendorCertificatesLines; VendorCertificatesLines) //  note para agregar lineas dentro de una pagina tipo documento. Importante que la pagina de lineas sea tipo ListPart. Una "pagina vinculada a otra tabla" dentro de otra pagina, es como un renderizado dentro de otro renderizado

            {
            }
        }
        area(FactBoxes)
        {
            systempart(Notes; Notes)
            {
            }
            systempart(Links; Links)
            {
            }
            systempart(MyNotes; MyNotes)
            {
           }
        }
    }
```


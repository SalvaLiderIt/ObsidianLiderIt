
Para agregar una subpágina, en este caso de tipo Linea, dentro de otra página y que muestre información desde una misma card, de la propia card y de otra "página" desde la misma interfaz hay que agregar, justo al terminar los grupos, como hermano en la página de tipo Document, la siguiente línea:

```al
part(VendorEvaluationListSubform; "VendorEvaluationListSubform") { } 
```
 Importante que la pagina de lineas sea tipo ListPart. Una "pagina vinculada a otra tabla" dentro de otra pagina, es como un renderizado dentro de otro renderizado

![[Pasted image 20251112105851.png]]

Para que aparezca la página de linea (tipo Listpart), en formato de tabla, en lugar de que tenga Group, tiene que tener "repeater"

Ejemplo Completo del Document que contiene la linea
```al
page 50101 VendorEvaluationCardDocument

{
    PageType = Document;
    ApplicationArea = All;
    UsageCategory = Administration;
    SourceTable = VendorEvaluationHeader;
    Caption = 'Vendor Evaluation Card';

    layout
    {
        area(Content)
        {
            group(GroupName)
            {
                Caption = 'General Information';
                field(EvaluationNo; Rec.EvaluationNo)
                {
                }
                field(EvaluationDate; Rec.EvaluationDate)
                {
                }
                field(ArchiveDate; Rec.ArchiveDate)
                {
                }
                field(Blocked; Rec.Blocked)
                {
                }
                field(VendorNo; Rec.VendorNo)
                {
                }
                field(VendorName; Rec.VendorName)
                {
                }
                field(OtherAspects; Rec.OtherAspects)
                {
                    MultiLine = true; //esto permite que el campo sea más grande
                }
                field(Result; Rec.Result)
                {
                }
                field(Responsible; Rec.Responsible)
                {
                }
            }
            part(VendorEvaluationListSubform; "VendorEvaluationListSubform") //  note para agregar lineas dentro de una pagina tipo documento. Importante que la pagina de lineas sea tipo ListPart. Una "pagina vinculada a otra tabla" dentro de otra pagina, es como un renderizado dentro de otro renderizado

            {

                SubPageLink = EvaluationNo = field(EvaluationNo);

            }
        }
    }
}
```

Ejemplo completo de la subpágina linea-line de tipo ListPart que se encuentra dentro del anterior Document

```al
page 50102 VendorEvaluationListSubform

{
    PageType = ListPart;
    ApplicationArea = All;
    UsageCategory = Administration;
    SourceTable = VendorEvaluationLines;
    Caption = 'Vendor Evaluation Lines Subform';
  
    layout
    {
        area(Content)
        {
            repeater(GroupName)
            {
                Caption = 'Vendor Evaluation Lines';
                field(Criteria; Rec.Criteria)
                {
                }
                field(Good; Rec.Good)
                {
                }
                field(Normal; Rec.Normal)
                {
                }
                field(Bad; Rec.Bad)
                {
                }
            }
        }
    }
}
```

En concreto, la propiedad SubPageLink (dentro de part en la page document), se utiliza para aplicar directamente un filtro del padre directo al hijo indicado
```al
SubPageLink = EvaluationNo = field(EvaluationNo);
```
- Evita tener que hacer filtros manuales.
- Permite que el usuario vea directamente las líneas asociadas al documento que está editando.
- Es fundamental en páginas tipo documento (cabecera + líneas), como órdenes de compra, facturas, evaluaciones, etc.


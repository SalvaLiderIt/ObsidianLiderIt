

```al
page 50101 "Taste Card"
{
    PageType = Card;
    ApplicationArea = All;
    UsageCategory = Administration;
    SourceTable = Taste;
    Caption = 'Taste Card';

    layout
    {
        area(Content)
        {
            group(FlavourCard)
            {
                Caption = 'Flavour Card';
                field(Code; Rec.Code)
                {

                }

                field(Description; Rec.Description)
                {

                }
                field(ProductCountDisplay; Rec.GetProductCountDisplay())
                {
                    Caption = 'Taste Products Count';
                    ToolTip = 'Specifies Number of products with this taste.';
                }
            }
        }
    }
}
```



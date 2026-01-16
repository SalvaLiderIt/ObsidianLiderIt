Para que un producto sea reconocible por el escáner, previamente hay que configurar cada producto desde la página Productos --> Seleccionamos el producto en cuestión (o lo creamos) --> Más Opciones --> Relacionado --> Producto --> Referencias Artículo --> 

![[Pasted image 20260116125020.png]]


# ✅ 1. Qué es realmente el campo “N.º referencia” en BC

En **Movs. ref. art. → Tipo referencia = Cód. barras**, el campo **N.º referencia** NO espera:

- ni el DataMatrix completo
- ni el lote (AI 10)
- ni la fecha de caducidad (AI 17)
- ni el serial (AI 21)

**Debe contener únicamente el identificador del producto** que es común a todas las unidades de ese mismo artículo.

👉 **En GS1, ese identificador es el GTIN**, que corresponde al AI **(01)**.

Es decir:  
El **N.º referencia = GTIN**.

---

# ✅ 2. Extraemos el GTIN de tus tres códigos DataMatrix

Aquí están tus códigos:

### ✔ Código 1

```
01084700066159801728033110BX593221VG4R5TWP1
```

GTIN = **08470006615980**

---

### ✔ Código 2

```
01084700068872951726103110BX442621M634PX6LP
```

GTIN = **08470006887295**

---

### ✔ Código 3

```
01084700070981881727022810LC9026321K9NPS76HAY
```

GTIN = **08470007098188**

Para obtener el GTIN de cada código escaneado hace falta eliminar los dos primeros dígitos, y nos quedamos con las posiciones 3....16, un total de 14 dígitos.
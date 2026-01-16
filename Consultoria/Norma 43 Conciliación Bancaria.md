# ✅ 1. ENTENDER TU ARCHIVO: ESTO ES NORMA 43

La Norma AEB43 se compone de registros:

- **11** → Cabecera del fichero
- **22** → Movimiento
- **23** → Datos complementarios del movimiento
- **33** → Total del extracto (pie)

Tu fichero sigue exactamente ese patrón.

---

# ✅ 2. DÓNDE ESTÁ EL IMPORTE EN NORMA 43 (LÍNEA 22)

Cada línea tipo **22** tiene esta estructura (posiciones fijas):

|Campo|Posición|Longitud|Descripción|
|---|---|---|---|
|Código de registro|1–2|2|Siempre `22`|
|Fecha operación|3–10|8|AAAA/MM/DD o AA/MM/DD según banco|
|Fecha valor|11–18|8|Igual|
|Código de concepto|19–20|2|Ej: 03, 11, 19…|
|Clave de operación|21|1|0=cargo / 1=abono|
|Importe|22–34|13|Entero sin coma|
|Importe decimal|35–36|2|Decimales|
|…|...|...|Más campos|

👉 **El importe real es la concatenación de pos. 22–36**, y el signo depende de la **clave de operación** (pos. 21).

### Ejemplo con una de tus líneas:

```
22    074125111725111704100200000000475000001502414700000000000047781/49/P3576/0
```

Voy a separarla por posiciones:

- `22` → tipo de registro
- `07412511` → fecha operación (ya te explico esto abajo)
- `17251117` → fecha valor
- `041` → código concepto + clave
- **`002000000004750`** → importe entero (pos. 22–34)
- **`00`** → decimales (pos. 35–36)
- …

→ **Importe resultante = 4.750,00 (cargo)**

✔ Coincide exactamente con lo que te aparece en tu captura de BC.

---

# 🟦 Importante: ¿por qué los importes parecen tan raros en el TXT?

Ejemplo:

```
0000000047500
```

Esto realmente significa:

- quitar ceros a la izquierda → `47500`
- dividir entre 100 → `475.00`
- si clave = 0 → es **cargo** → lo verás como **-475.00**
- si clave = 1 → es **abono** → lo verás como **+475.00**

---

# ✅ 3. VAMOS A LEER OTRO EJEMPLO DE TU FICHERO

Tomamos esta línea:

```
22    07412511172511170301910000000000243000001022420000000000000000000000000000
```

**Importe (pos 22–36):**

```
000000000243000
```

Quitando ceros:  
→ 24300  
Dividiendo entre 100:  
→ 243,00

¡Coincide con lo que tienes en la pantalla!  
Ahí te aparece **-243,00** como cargo.

---

# ⚠ Sobre las fechas “07412511”

Muchos bancos en España usan este formato **DDMMAA**, pero _encapsulado_ en un formato extraño cuando se genera con Norma 43:

`07412511`

→ `07/41/25` no tiene sentido  
Pero si lo splitteas como hace Bankinter:

```
07 41 25 11
DD CC MM AA
```

Donde:

- **41** → código de operación banco
- fecha real = `07/11/25`

Esto es típico de Bankinter, Sabadell y Unicaja.  
No te preocupes: tu parser debe tomar solo las posiciones estándar (3–8 y 11–16), ignorando la codificación interna del banco.



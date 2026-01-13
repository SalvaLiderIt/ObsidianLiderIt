
## 📘 Lista de conceptos clave de contabilidad en Business Central

### 🔹 Documentos de Compras y Ventas

- **Oferta de compra**: Documento preliminar para solicitar condiciones a un proveedor. `Purchase Quote` (ID 51)
- **Pedidos compra**: Solicitud formal de productos/servicios a un proveedor. `Purchase Order` (ID 50)
- **Factura de compra**: Documento legal que refleja la transacción de compra. `Purchase Invoice` (ID 51)
- **Nota de crédito de compra**: Corrección o devolución de una factura de compra. `Purchase Credit Memo` (ID 53)

- **Oferta de venta**: Documento preliminar para ofrecer condiciones a un cliente. `Sales Quote` (ID 9300)
- **Pedido de venta**: Solicitud formal de un cliente para adquirir productos/servicios. `Sales Order` (ID 42)
- **Factura de venta**: Documento legal que refleja la transacción de venta. `Sales Invoice` (ID 43)
- Abono de venta: Corrección o devolución de una factura de venta. `Sales Credit Memo` (ID 44) Es el documento que se utiliza para registrar devoluciones o correcciones de facturas de venta.

- **Cabecera de pedido (compra/venta)**: Contiene datos generales del documento (cliente/proveedor, fechas, condiciones). `Sales Header` / `Purchase Header` (Tabla 36 / Tabla 38)
- **Líneas de pedido (compra/venta)**: Detallan productos, cantidades, precios y descuentos. `Sales Line` / `Purchase Line` (Tabla 37 / Tabla 39)

---

### 🔹 Contabilidad General

- **Plan de cuentas**: Estructura de todas las cuentas contables. `Chart of Accounts` (Page 16, Table 15)
- **Asientos contables**: Registro de cada movimiento financiero. `General Ledger Entries` (Page 20, Table 17)
- **Diarios generales**: Herramienta para registrar movimientos contables manuales. `General Journal` (Page 39, Table 81)
- **Grupos contables**: Configuración que define cómo se registran las transacciones. `General Posting Setup` (Page 252, Table 252)

---

### 🔹 Estados Financieros

- **Balance de situación**: Muestra activos, pasivos y patrimonio neto. `Balance Sheet` (Report 10021)
- **Estado de resultados (P&L)**: Refleja ingresos, gastos y beneficios/pérdidas. `Income Statement` (Report 10022)
- **Flujos de efectivo**: Seguimiento de entradas y salidas de dinero. `Cash Flow Forecast` (Page 841, Table 841)

---

### 🔹 Gestión de Inventario y Almacén

- **Recepción de mercancías**: Registro de entrada de productos comprados. `Posted Purchase Receipt` (Page 136, Table 109)
- **Albarán de entrega**: Documento que confirma la salida de mercancías hacia el cliente. `Posted Sales Shipment` (Page 142, Table 110)
- **Valoración de inventario**: Métodos de cálculo del valor de stock. `Inventory Valuation` (Report 1001)

---

### 🔹 Ciclo de Compras y Ventas

- **Pagos a proveedores**: Liquidación de facturas de compra. `Vendor Ledger Entries` (Page 25, Table 25)
- **Cobros de clientes**: Liquidación de facturas de venta. `Customer Ledger Entries` (Page 25, Table 21)
- **Conciliación bancaria**: Cuadre de movimientos contables con extractos bancarios. `Bank Acc. Reconciliation` (Page 375, Table 274)

---

### 🔹 Otros conceptos esenciales

- **IVA e impuestos**: Configuración de tipos impositivos. `VAT Posting Setup` (Page 325, Table 325)
- **Dimensiones**: Etiquetas para clasificar movimientos (ej. departamento, proyecto). `Dimensions` (Page 536, Table 348)
- **Periodificación**: Distribución de ingresos/gastos en varios periodos. `Accrual Scheme` (Page 170, Table 170)


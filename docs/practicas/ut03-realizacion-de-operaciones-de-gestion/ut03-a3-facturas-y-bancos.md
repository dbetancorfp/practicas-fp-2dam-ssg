# UT03-A3 - Facturas y Bancos (Odoo 17)

## 1. Resultado de aprendizaje, contenido y criterios de evaluación

### Resultado de aprendizaje
RA3. Realiza operaciones de gestión, consulta y análisis de la información siguiendo las especificaciones de diseño y utilizando las herramientas proporcionadas por los sistemas ERP-CRM

### Contenido
4. Informes y listados de la aplicación

### Criterios de evaluación
c) Se han generado informes.
d) Se han exportado datos e informes.

## 2. Agrupamiento

Individual

## 3. Contexto

Con los terceros y productos de la práctica A1 y los presupuestos y pedidos de la práctica A2 ya registrados en Odoo, toca cerrar el ciclo económico de la empresa: facturar, configurar las cuentas de tesorería (banco y caja), cobrar a clientes, pagar a proveedores y comprobar que el stock y los saldos cuadran tras cada operación. Al final se generan y exportan los informes de facturación.

## 4. Objetivo de la actividad

Practicar en Odoo la emisión de facturas a clientes y proveedores (desde un pedido, desde un reaprovisionamiento y desde cero), la configuración de los diarios de banco y efectivo con sus saldos iniciales, el registro de cobros y pagos, la corrección de existencias y la obtención y exportación de informes.

## 5. Enunciado de la actividad

Realiza un informe con pantallazos de las tareas siguientes. Cada vez que aparezca 📸, incluye una captura en el informe.

> **Cómo leer las rutas.** `Ventas ▸ Pedidos ▸ Presupuestos` significa: menú de aplicaciones (icono de cuadrícula ▦, arriba a la izquierda) → aplicación **Ventas** → menú superior **Pedidos** → opción **Presupuestos**. La aplicación de facturación aparece en el menú como **Facturación / Contabilidad**; aquí se abrevia **Facturación**.
>
> **Datos de ejemplo.** Los nombres, importes y números de documento (S00002, INV/2026/00002, P00001…) son los de una ejecución de referencia. Usa tus propios datos de A1 y A2; los números de documento que te asigne Odoo pueden ser distintos.

### 5.0. Preparación (prerrequisitos)

**a) Aplicaciones.** En `▦ ▸ Aplicaciones` comprueba que están instaladas (botón **Activar** si no lo están) **Ventas**, **Compra**, **Inventario** y **Facturación**. La instalación de Inventario tarda unos segundos; al terminar, la página se recarga sola.

**b) Datos maestros (práctica A1).** Si te faltan, crea al menos:

| Tipo | Ruta | Ejemplo |
| :--- | :--- | :--- |
| Cliente (empresa) | `Ventas ▸ Pedidos ▸ Clientes ▸ Nuevo` (marcar **Empresa**) | *Cafetería La Plaza SL* |
| Segundo cliente | ídem | *Hotel Teide Mar SA* |
| Proveedor (empresa) | `Compra ▸ Pedidos ▸ Proveedores ▸ Nuevo` | *Suministros Canarias SL* |
| Producto almacenable | `Inventario ▸ Productos ▸ Productos ▸ Nuevo`, **Tipo de producto = Producto almacenable** | *Café en grano 1 kg* (venta 18 €, coste 10 €) · *Taza de cerámica* (venta 6 €, coste 2,50 €) |
| Servicio | ídem, **Tipo de producto = Servicio** | *Mantenimiento de cafetera* (60 €) · *Curso de barista (2 h)* (90 €) |

- En la pestaña **Compra** de cada producto almacenable, añade el proveedor con su precio de compra: así Odoo lo propondrá automáticamente al reaprovisionar.
- Asigna a los servicios el impuesto **21% S** (Servicios) en la pestaña **Información general** → *Impuestos del cliente*. Si no lo haces, Odoo les aplica por defecto *21% G (Bienes)*.
- Un producto almacenable necesita **existencias** para poder entregarse (si no, la entrega se queda *En espera*): `Inventario ▸ Operaciones ▸ Ajustes ▸ Inventario físico` → **Nuevo** → producto, ubicación `WH/Stock`, **Cantidad contada** → **Aplicar**. Ejemplo: 20 kg de café y 50 tazas.

**c) Presupuesto aceptado (práctica A2).** Debe existir un presupuesto en estado **Presupuesto enviado** con al menos un producto almacenable y un servicio, y con las **Condiciones de pago** rellenas. Ejemplo: **S00002** para *Cafetería La Plaza SL*, 5 × Café en grano (90 €) + 1 × Mantenimiento de cafetera (60 €), condiciones **Pago inmediato**, nota "Pago a la entrega con tarjeta". Total **181,50 €** (IVA 21 %).

### 5.1. Factura desde el presupuesto aceptado e intento de pago

**Confirmar el presupuesto.**
1. `Ventas ▸ Pedidos ▸ Presupuestos` → abre el presupuesto (**S00002**).
2. Pulsa **Confirmar**. La barra de estado pasa a **Pedido de venta** y aparece el botón inteligente **🚚 Entrega 1**. 📸

**Entregar antes de facturar.** ⚠️ Con Inventario instalado, la **Política de facturación** por defecto de los productos almacenables es **Cantidades entregadas**: solo se facturan las unidades ya entregadas. Si facturas antes de entregar, la factura sale **únicamente con el servicio** (72,60 € en lugar de 181,50 €).

> Alternativa para una "facturación inmediata": en la ficha del producto, pestaña **Información general**, cambia la **Política de facturación** a **Cantidades pedidas**.

3. En el pedido, pulsa **🚚 Entrega**. Se abre el albarán **WH/OUT/0000x** con *Disponibilidad del producto: Disponible*.
4. Pulsa **Validar**; el estado pasa a **Hecho**. 📸
5. Vuelve al pedido desde la miga de pan (**S00002**, arriba a la izquierda). La columna **Entregado** muestra ya 5,00.

**Crear la factura (100 %).**
6. En el pedido, pulsa **Crear factura**. En la ventana *Crear facturas* deja marcada **Factura normal** (factura el 100 % de lo pedido/entregado; las otras opciones son *Anticipo (porcentaje)* y *Anticipo (importe fijo)*). 📸
7. Pulsa **Crear borrador de factura**. El borrador incluye las dos líneas y **hereda del presupuesto** las **Condiciones de pago** (*Pago inmediato*) y la nota "Pago a la entrega con tarjeta". Total 181,50 €. 📸

**Validar la factura y obtener el PDF.**
8. Pulsa **Confirmar**. La factura recibe número (**INV/2026/0000x**) y pasa a **Publicado**. 📸
9. Pulsa **Enviar e imprimir**. En la ventana *Enviar*, la casilla **Descargar** genera el PDF y **Correo electrónico** lo envía al cliente con el adjunto `INV_2026_0000x.pdf`. Desmarca *Correo electrónico* si solo quieres el PDF. **Vista previa** muestra la factura tal como la ve el cliente. 📸

**Intentar registrar el pago.**
10. Pulsa **Registrar pago**. Se abre *Registrar pago* con **Diario = Banco**, **Importe = 181,50 €** y **Cuenta bancaria destinataria** vacía. 📸
11. **No crees el pago todavía**: pulsa **Descartar**.

> **Diferencia de Odoo con otros ERP.** En Odoo 17 con la localización española **no aparece un error por falta de banco**: al instalar Facturación se crean automáticamente los diarios **Banco** (cuenta contable *572001 Banco*) y **Efectivo** (*570001 Efectivo*). Lo que falta es asociar al diario Banco una cuenta bancaria real (IBAN). Explica en el informe esta diferencia apoyándote en la captura del paso 10. La configuración se completa en el apartado 5.2.

### 5.2. Cuenta bancaria y caja/efectivo con sus saldos

En Odoo cada cuenta de tesorería es un **diario** de tipo *Banco* o *Efectivo* asociado a una cuenta contable del grupo 57. Ambos diarios ya existen; solo hay que completarlos.

**Asociar el IBAN al diario Banco.**
1. `Facturación ▸ Configuración ▸ Contabilidad ▸ Diarios` → abre **Banco**.
2. En la pestaña **Asientos contables**, bloque *NÚMERO DE CUENTA BANCARIA*:
   - **Número de cuenta**: escribe el IBAN (p. ej. `ES9121000418450200051332`) y elige **Crear "ES91…"** en el desplegable.
   - **Banco** (aparece al rellenar el anterior): escribe el nombre (p. ej. `CaixaBank`) → **Crear "CaixaBank"**.
3. Guarda con el icono de la **nube** ☁ junto al título. El cambio queda registrado en el chatter (panel derecho). 📸

> Para crear una cuenta bancaria **nueva** en lugar de completar la existente: `Facturación ▸ Configuración ▸ Bancos ▸ Añadir una cuenta bancaria`. El diario **Efectivo** no necesita IBAN.

**Registrar los saldos iniciales (asiento de apertura).**
4. Abre la aplicación **Facturación**. Su pantalla inicial es el **Tablero de Contabilidad**, con las tarjetas *Facturas de cliente*, *Facturas de proveedores*, *Operaciones varias*, *Banco* y *Efectivo* (en adelante, `Facturación ▸ Tablero`).
5. En la banda superior de consejos de incorporación, bloque **Plan contable**, pulsa **Revisión**. Se abre el **Plan contable** con las columnas **Débito de apertura** y **Crédito de apertura**.
   > ⚠️ No cierres esa banda con la **✕**: en Facturación (Community) el *Plan contable* no tiene entrada propia en el menú y esta es la forma más directa de llegar a él.
6. Busca `570001` (Intro), haz clic en la celda **Débito de apertura** de *Efectivo*, escribe `500` y pulsa **Guardar** (arriba a la izquierda).
7. Busca `572001` y pon **Débito de apertura** = `10000` en *Banco* → **Guardar**.
8. Busca `100000` (*Capital social*) y pon **Crédito de apertura** = `10500` → **Guardar**. Así el asiento cuadra (500 + 10.000 al debe = 10.500 al haber); si omites este paso, Odoo cuadra automáticamente contra *129000 Resultado del ejercicio*. 📸
9. Odoo genera el **Asiento contable de apertura** en estado **Borrador** y hay que publicarlo: `Facturación ▸ Tablero` → tarjeta **Operaciones varias** → menú **⋮** (arriba a la derecha de la tarjeta) → **Asientos contables** → abre *Vario/…/0001 (Asiento contable de apertura)* → **Publicar**. 📸
10. Vuelve al Tablero: las tarjetas muestran **Banco 10.000,00 €** y **Efectivo 500,00 €**. 📸

**Saldo mínimo autorizado y saldo mínimo deseado.** Odoo **no dispone de campos** para estos límites (son propios de otros ERP, como Dolibarr). Para dejar constancia en el sistema:

11. En el diario **Banco** (`Facturación ▸ Configuración ▸ Contabilidad ▸ Diarios`), pulsa **Registrar una nota** en el chatter y escribe, por ejemplo: `Saldo inicial: 10.000 €. Saldo mínimo autorizado: -1.000 € (descubierto pactado con el banco). Saldo mínimo deseado: 3.000 €.` → **Registrar**. 📸
12. Repite en **Efectivo**: `Saldo inicial: 500 €. Saldo mínimo autorizado: 0 € (la caja no admite saldo negativo). Saldo mínimo deseado: 200 €.` 📸

Explica en el informe que, en Odoo, el control de esos mínimos se hace consultando el saldo de la tarjeta de cada diario en el Tablero.

### 5.3. Cobro de la primera factura por caja/efectivo

1. `Facturación ▸ Clientes ▸ Facturas de clientes` → abre la factura del apartado 5.1 (estado *Publicado*, *No pagado*).
2. Pulsa **Registrar pago** y completa la ventana:
   - **Diario**: **Efectivo** (por defecto aparece *Banco*; observa que *Cuenta bancaria destinataria* ya muestra el IBAN configurado en 5.2).
   - **Método de pago**: Manual · **Importe**: 181,50 € · **Fecha de pago**: hoy · **Memo**: número de factura. 📸
3. Pulsa **Crear pago**. La factura muestra la banda verde **PAGADA** y el chatter registra *No pagado → Pagada*. 📸
4. Comprueba el ingreso en `Facturación ▸ Tablero` → tarjeta **Efectivo**: *Pagos 181,50 €* + *Operaciones varias 500,00 €* = **681,50 €** en caja. 📸
   - El enlace **Pagos** de la tarjeta muestra el detalle del cobro.
   - Listado general de cobros: `Facturación ▸ Clientes ▸ Pagos`.

> **Cómo leer las tarjetas del Tablero.** Cada tarjeta de Banco o Efectivo separa *Operaciones varias* (saldo inicial) y *Pagos* (cobros en positivo, pagos en negativo); el saldo disponible es la suma de ambas. Odoo deja los cobros y pagos en una cuenta transitoria (*cobros/pagos pendientes*) hasta conciliarlos con el extracto bancario; por eso la cuenta 570001/572001 por sí sola sigue mostrando solo el saldo inicial.

### 5.4. Factura de proveedor desde un reaprovisionamiento, pago por banco y corrección de stock

**Lanzar el reaprovisionamiento.**
1. `Inventario ▸ Productos ▸ Productos` → abre un producto almacenable con proveedor en su pestaña **Compra** (ejemplo: **Taza de cerámica**, stock 50).
2. Pulsa **Reponer**. En la ventana *¿Bajas existencias? Vamos a reponer.* indica **Cantidad** `100`; **Ruta preferida** (*Comprar*) y **Proveedor** (*Suministros Canarias SL*) los propone Odoo a partir de la pestaña Compra. 📸
3. Pulsa **Confirmar**. Odoo crea una **Solicitud de presupuesto** (SdP) al proveedor con origen *Reposición manual*.

> Alternativa: `Inventario ▸ Operaciones ▸ Abastecimiento ▸ Reposición`, que trabaja con reglas de reabastecimiento mínimo/máximo.

**Confirmar el pedido de compra y recibir la mercancía.**
4. `Compra ▸ Pedidos ▸ Solicitudes de presupuesto` → abre la nueva SdP (**P0000x**: 100 × 2,50 € = 250 € + IVA = **302,50 €**). 📸
5. Pulsa **Confirmar pedido**. El estado pasa a **Pedido de compra** y aparece el botón **🚚 Recepción 1**.
6. Pulsa **Recibir productos** → en la recepción **WH/IN/0000x** (*Demanda 100, Cantidad 100*) pulsa **Validar**; el estado pasa a **Hecho**. 📸
7. Vuelve al pedido desde la miga de pan: la columna **Recibido** muestra 100.

> ⚠️ Igual que en ventas, en compras se factura por defecto lo **recibido**: si creas la factura antes de validar la recepción, sale sin líneas.

**Crear y validar la factura de proveedor.**
8. En el pedido, pulsa **Crear factura**. Se abre un borrador de *Factura de proveedor* (**FACTU/2026/11/000x**) con la línea del pedido.
9. Rellena la **Referencia de factura** (número de la factura del proveedor, p. ej. `SC-2026-0457`) y la **Fecha de la factura**, que **es obligatoria** para poder confirmar.
10. Pulsa **Confirmar**; el estado pasa a **Publicado**. 📸

**Pagar la factura con la cuenta bancaria.**
11. Pulsa **Registrar pago** → **Diario = Banco** (aparece por defecto), **Importe = 302,50 €** → **Crear pago**. La factura muestra la banda **PAGADA**. 📸
12. Comprueba el pago en `Facturación ▸ Tablero` → tarjeta **Banco**: *Pagos −302,50 €* y *Operaciones varias 10.000,00 €*, es decir, **9.697,50 €**. Listado de pagos a proveedores: `Facturación ▸ Proveedores ▸ Pagos`. 📸

**Comprobar y corregir el stock.**
13. `Inventario ▸ Operaciones ▸ Ajustes ▸ Inventario físico`: la línea de *Taza de cerámica* muestra **Cantidad real = 150** (50 iniciales + 100 recibidas). 📸
14. Simula una merma de 2 tazas rotas al desembalar: en la celda **Cantidad contada** escribe `148` → **Guardar**. La columna **Diferencia** muestra **−2,00** en rojo. 📸
15. Pulsa **Aplicar** en la línea; la **Cantidad real** pasa a **148**. 📸
16. Consulta el movimiento con el botón **Historial** de la línea (o en `Inventario ▸ Informes ▸ Historial de movimientos`): aparecen la recepción *WH/IN/0000x* (+100) y el ajuste *Cantidad de producto actualizada* (WH/Stock → *Inventory adjustment*, 2 uds). 📸

### 5.5. Factura a un cliente creada desde cero

La factura se crea directamente en Facturación, sin presupuesto ni pedido previo.

1. `Facturación ▸ Clientes ▸ Facturas de clientes` → **Nuevo**.
2. En la cabecera:
   - **Cliente**: escribe `Hotel Teide` y pulsa Intro para elegir *Hotel Teide Mar SA*.
   - **Fecha de factura**: déjala vacía (Odoo pone la de hoy al confirmar) o indica una fecha.
   - **Condiciones de pago**: en el desplegable situado junto a *Fecha de vencimiento* elige **15 días**; el campo pasa a llamarse *Condiciones de pago* y desaparece la fecha de vencimiento manual. 📸
3. En la pestaña **Líneas de factura**, pulsa **Añadir una línea**:
   - `Curso de barista` (Intro) → **Cantidad** `2` (el precio de 90 € se rellena solo).
   - **Añadir una línea** → `Mantenimiento` (Intro) → cantidad 1 (60 €).
   - Total: 240 € + IVA 50,40 € = **290,40 €**. 📸
4. Pulsa **Confirmar**: la factura recibe número (**INV/2026/0000x**) y pasa a **Publicado**. 📸
5. Obtén el PDF con **Enviar e imprimir** (como en 5.1). El texto del correo propuesto incluye la cuenta bancaria configurada en 5.2 para que el cliente pague por transferencia.
6. Pulsa **Registrar pago** → **Diario = Banco** (la *Cuenta bancaria destinataria* muestra *ES91 2100 … CaixaBank (confiable)*) → **Crear pago** → banda **PAGADA**. 📸
7. Comprueba la tarjeta **Banco** del Tablero: *Pagos* = −302,50 + 290,40 = **−12,10 €**; saldo total **9.987,90 €**. 📸

> Una factura creada desde cero **no mueve stock**, aunque incluya productos almacenables, porque no hay pedido ni albarán. Por eso aquí se facturan servicios. Para vender un producto almacenable hay que pasar por Ventas (apartado 5.1) y generar la entrega.

### 5.6. Simulación del ciclo completo con todos los productos y servicios

Repite los flujos anteriores con **todos** tus productos y servicios, tanto en compra a proveedores como en venta a clientes.

**Compra de productos y de un servicio.**
1. `Compra ▸ Pedidos ▸ Solicitudes de presupuesto` → **Nuevo**:
   - **Proveedor**: *Suministros Canarias SL*; en la pestaña **Otra información**, **Condiciones de pago** = *30 días*.
   - Líneas (**Añadir un producto**): 20 × *Café en grano 1 kg* (10 €), 30 × *Taza de cerámica* (2,50 €) y 1 × *Curso de barista (2 h)* a 50 € con la descripción *Formación de barista impartida por proveedor* (servicio subcontratado).
   - Total: 325 € + IVA = **393,25 €**. 📸
2. **Confirmar pedido** → **Recibir productos**. La recepción **WH/IN/0000x** contiene solo el café y las tazas (los servicios no pasan por el almacén) → **Validar**. 📸
3. Vuelve al pedido. ⚠️ El servicio muestra **Recibido = 0,00**: en los servicios la cantidad recibida se introduce **a mano**. Haz clic en la celda **Recibido** de la línea del curso, escribe `1` y guarda (☁). Si no lo haces, la factura sale sin el servicio. 📸
4. **Crear factura**: el borrador trae las **3 líneas** y las *Condiciones de pago = 30 días* del pedido. Rellena **Fecha de la factura** y **Referencia de factura** → **Confirmar** → **Registrar pago** (*Banco*) → **PAGADA**. 📸

**Venta de productos y servicios.**
5. `Ventas ▸ Pedidos ▸ Presupuestos` → **Nuevo**: cliente *Hotel Teide Mar SA*, **Condiciones de pago** *15 días*; líneas: 10 × Café, 24 × Taza, 1 × Mantenimiento de cafetera y 1 × Curso de barista. Total: 474 € + IVA = **573,54 €**. 📸
6. **Confirmar** → **🚚 Entrega** → **Validar** (salen 10 kg de café y 24 tazas). 📸
7. Vuelve al pedido → **Crear factura** → *Factura normal* → **Crear borrador de factura** → **Confirmar** → **Registrar pago** (*Banco*) → **PAGADA**. 📸

**Comprobaciones finales.** Haz una captura de cada pantalla. Los valores de referencia corresponden a los datos de ejemplo: 📸

| Qué comprobar | Dónde | Valor de referencia |
| :--- | :--- | :--- |
| Facturas de cliente | `Facturación ▸ Clientes ▸ Facturas de clientes` | 181,50 €, 290,40 € y 573,54 €, todas **Pagada / Publicado** |
| Facturas de proveedor | `Facturación ▸ Proveedores ▸ Facturas de proveedores` | 302,50 € y 393,25 €, ambas **Pagada** |
| Saldo de caja | `Facturación ▸ Tablero` → tarjeta **Efectivo** | Pagos 181,50 + Operaciones varias 500 = **681,50 €** |
| Saldo de banco | `Facturación ▸ Tablero` → tarjeta **Banco** | Pagos 168,19 (= −302,50 + 290,40 − 393,25 + 573,54) + 10.000 = **10.168,19 €** |
| Stock | `Inventario ▸ Operaciones ▸ Ajustes ▸ Inventario físico` | Café 20 − 5 + 20 − 10 = **25** · Tazas 50 + 100 − 2 + 30 − 24 = **154** |
| Movimientos de almacén | `Inventario ▸ Operaciones ▸ Traslados ▸ Recepciones` / `… ▸ Entregas` | Todas las recepciones (WH/IN) y entregas (WH/OUT) en **Hecho** |

> Si en el listado de facturas te aparece una factura **Cancelado**, corresponde a un intento de facturar antes de entregar (ver 5.1). Siguiendo el orden indicado no se genera.

### 5.7. Informes y exportación de datos

**Informe de facturación.**
1. `Facturación ▸ Informes ▸ Administración ▸ Análisis de facturas`. Se abre en vista de **gráfico** con los filtros *Facturado* y *Clientes*.
2. Cambia a la vista **Tabla dinámica** (icono de cuadrícula, arriba a la derecha).
3. Haz clic en la cabecera de fila **⊟ Total** (se pliega) y de nuevo en **⊞ Total** → elige **Empresa**. Con los datos de ejemplo: *Cafetería La Plaza SL 150,00* · *Hotel Teide Mar SA 714,00* · **Total 864,00** (base imponible, sin IVA). 📸
4. Sustituye el filtro *Clientes* por *Proveedores* (desplegable ▾ del buscador → *Filtros*) para obtener el informe de compras. 📸
5. Exporta el informe con el botón **⤓ Descargar xlsx** (tercer icono junto a *Medidas*) y adjunta una captura del fichero abierto. 📸

**Exportación de listados.**
6. `Facturación ▸ Clientes ▸ Facturas de clientes` (vista de lista): marca la casilla de la cabecera para seleccionar todas las facturas (aparece *N seleccionado*).
7. **⚙ Acciones ▸ Exportar**. En la ventana *Exportar información*:
   - **Formato de exportación**: *XLSX* o *CSV*.
   - **Campos a exportar**: vienen preseleccionados Número, Fecha de factura, Estado de pago, Total…; añade más con **+** desde *Campos disponibles* o quítalos con 🗑.
   - Pulsa **Exportar** para descargar el fichero. 📸
8. El mismo menú **Acciones** ofrece **Exportar ZIP** (PDF de todas las facturas seleccionadas) y **Enviar e imprimir** masivo. **Acciones ▸ Exportar** funciona igual en cualquier lista de Odoo (pagos, pedidos de compra, productos, inventario físico).

### Resumen de rutas

| Operación | Ruta en Odoo 17 |
| :--- | :--- |
| Presupuestos / pedidos de venta | `Ventas ▸ Pedidos ▸ Presupuestos` · `Ventas ▸ Pedidos ▸ Pedidos` |
| Facturar un pedido | Pedido → **Crear factura** → *Factura normal* |
| Facturas de cliente | `Facturación ▸ Clientes ▸ Facturas de clientes` |
| Facturas de proveedor | `Facturación ▸ Proveedores ▸ Facturas de proveedores` |
| Cobros / pagos | `Facturación ▸ Clientes ▸ Pagos` · `Facturación ▸ Proveedores ▸ Pagos` |
| Diarios de banco y caja | `Facturación ▸ Configuración ▸ Contabilidad ▸ Diarios` |
| Nueva cuenta bancaria | `Facturación ▸ Configuración ▸ Bancos ▸ Añadir una cuenta bancaria` |
| Saldos iniciales | `Facturación ▸ Tablero` → banda *Plan contable* → **Revisión** |
| Publicar el asiento de apertura | `Facturación ▸ Tablero` → *Operaciones varias* **⋮** → *Asientos contables* |
| Reaprovisionar | Ficha del producto → **Reponer** · `Inventario ▸ Operaciones ▸ Abastecimiento ▸ Reposición` |
| Pedidos de compra | `Compra ▸ Pedidos ▸ Solicitudes de presupuesto` · `Compra ▸ Pedidos ▸ Pedidos de compra` |
| Recepciones / entregas | Botón **Recepción** / **Entrega** del pedido · `Inventario ▸ Operaciones ▸ Traslados` |
| Corrección de stock | `Inventario ▸ Operaciones ▸ Ajustes ▸ Inventario físico` |
| Informe de facturas | `Facturación ▸ Informes ▸ Administración ▸ Análisis de facturas` |
| Exportar cualquier lista | Seleccionar registros → **⚙ Acciones ▸ Exportar** |

## 6. Material proporcionado al alumnado

- Instalación de **Odoo 17 Community** con localización española y las aplicaciones Ventas, Compra, Inventario y Facturación, accesible en `http://127.0.0.1:8069/web`.
- Base de datos con los terceros, productos, presupuestos y pedidos de las prácticas UT03-A1 y UT03-A2.

## 7. Entrega

- Informe en PDF con los pantallazos de todas las operaciones (📸) y del estado de las cuentas de banco y caja tras cada una.
- Ficheros exportados en el apartado 5.7 (xlsx o csv).
- Formato del documento: PDF.
- **Nombre del fichero: tuNombre_Apellido1_Apellido2_UT03-A3.pdf**

## 8. Rúbrica

| Criterio | Excelente | Bien | Suficiente | Insuficiente |
| :--- | :--- | :--- | :--- | :--- |
| **1. Generación y validación de facturas**<br>(Apartados 5.1, 5.4 y 5.5 del enunciado)<br>[3,0 puntos] | Documenta perfectamente las facturas creadas desde el pedido de venta (con su entrega previa), desde el reaprovisionamiento y desde cero, incluida la captura del asistente de pago sin cuenta bancaria con la explicación de la diferencia de Odoo, y los PDF correspondientes.<br>**(3,0 pts)** | Documenta la mayoría de las facturas, pero omite la captura y explicación del intento de pago inicial o algún paso intermedio (entrega, recepción, referencia o fecha de la factura de proveedor).<br>**(2,2 pts)** | Genera algunas facturas, pero faltan evidencias de procesos clave (por ejemplo, la validación o la factura de proveedor completa).<br>**(1,5 pts)** | No genera correctamente las facturas solicitadas o no aporta capturas que demuestren el trabajo realizado.<br>**(0 pts)** |
| **2. Configuración de cuentas bancarias y caja**<br>(Apartado 5.2 del enunciado)<br>[2,0 puntos] | Asocia el IBAN al diario Banco, registra y publica el asiento de apertura con los saldos iniciales de banco y caja, y deja constancia de los saldos mínimos autorizados y deseados en ambos diarios, con capturas de cada paso.<br>**(2,0 pts)** | Configura ambas cuentas, pero omite evidenciar algún parámetro (IBAN, publicación del asiento de apertura o saldos mínimos) en al menos una de ellas.<br>**(1,5 pts)** | Configura solo una de las cuentas (banco o caja), o los saldos registrados y mostrados son incorrectos o incompletos.<br>**(1,0 pts)** | No configura las cuentas bancarias ni de caja, lo que impide el correcto registro posterior de los cobros y pagos.<br>**(0 pts)** |
| **3. Gestión de cobros, pagos, simulación del ciclo e informes**<br>(Apartados 5.3, 5.4, 5.6 y 5.7 del enunciado)<br>[3,0 puntos] | Registra de forma impecable el cobro por caja, el pago al proveedor por banco y la corrección de stock, completa la simulación con todos los productos y servicios (incluida la recepción manual del servicio comprado), verifica saldos y stock en la tabla final, y genera y exporta el informe de facturación y el listado de facturas.<br>**(3,0 pts)** | Registra los cobros y pagos principales, pero la simulación final es incompleta, falla al evidenciar saldos o stock tras alguna operación, o falta una de las exportaciones.<br>**(2,2 pts)** | Realiza cobros o pagos aislados, pero no completa la simulación global, no verifica los saldos de banco y caja o no genera ni exporta los informes.<br>**(1,5 pts)** | No completa el registro de cobros y pagos ni realiza la simulación del ciclo de compra a proveedores y venta a clientes.<br>**(0 pts)** |
| **4. Formato, estructura y evidencias del documento**<br>(Apartado 7 del enunciado)<br>[2,0 puntos] | El informe en PDF incluye todas las capturas organizadas tras cada operación, adjunta los ficheros exportados y el nombre sigue exactamente la norma: tuNombre_Apellido1_Apellido2_UT03-A3.pdf.<br>**(2,0 pts)** | El documento tiene buena estructura y capturas claras, pero el nombre del archivo tiene un ligero error tipográfico o el orden de las evidencias es algo confuso.<br>**(1,5 pts)** | Presenta fallos de formato notables, faltan varias capturas para seguir el hilo de las operaciones, o el nombre del archivo no sigue la norma establecida.<br>**(1,0 pts)** | Documento desestructurado, sin evidencias claras, o entregado en un formato distinto al PDF solicitado (por ejemplo, documento de Word).<br>**(0 pts)** |

### Penalización por Retraso en la Entrega

| Entrega de tarea | Entrega en plazo | Se retrasa 1 día | Se retrasa entre 2 y 3 días | Se retrasa entre 4 y 5 días | Se retrasa más de 5 días |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **Puntuación** | **0** | **-1** | **-2** | **-3** | **-5** |

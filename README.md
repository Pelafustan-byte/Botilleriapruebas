# Botillería San Pablo

Sistema comercial y operativo de Botillería San Pablo, Constitución. Integra vitrina pública, catálogo, inventario, compras, POS, caja, pagos, devoluciones, pedidos, clientes, fiscalización desacoplada, reportes y control operacional sobre PostgreSQL.

## Alcance de esta instalación

Esta instancia corresponde exclusivamente a **Botillería San Pablo** y opera con una sola sucursal habilitada: **Casa matriz**. Datos, sucursales, bodegas, cajas, credenciales o servicios pertenecientes a otros proyectos no forman parte de esta instalación.

## Arquitectura vigente

- Node.js 24 y Express.
- React/Vite para la aplicación y runtimes complementarios.
- PostgreSQL como fuente de verdad operacional.
- Extensiones idempotentes mediante archivos `*-bootstrap.cjs`.
- Railway para producción, base de datos, backup lógico y sandbox fiscal controlado.
- Webpay Plus mediante adaptador Transbank.
- Proveedores DTE y POS físico desacoplados mediante contratos de integración.

## Capacidades

- catálogo público derivado del inventario;
- productos con EAN/UPC, SKU y QR;
- Casa matriz, bodegas, transferencias e inventarios físicos;
- listas de precios, promociones y cotización autoritativa;
- cajas, turnos, movimientos y cierres;
- ventas POS con pagos idempotentes;
- Webpay de integración o producción configurada;
- devoluciones parciales o totales y restitución de stock;
- documentos tributarios mediante proveedor externo habilitado;
- conciliación de pagos y terminales POS;
- clientes, direcciones, consentimiento y mayoría de edad;
- pedidos para retiro o despacho, reservas, preparación por escaneo y seguimiento público;
- RBAC, auditoría, observabilidad, alertas y recuperación ante desastres.

## Desarrollo local

```bash
npm install
cp .env.example .env
npm run dev
```

Variables mínimas:

```text
DATABASE_URL=postgresql://...
ADMIN_EMAIL=...
ADMIN_PASSWORD=...
SESSION_SECRET=...
PUBLIC_BASE_URL=http://localhost:5173
```

En producción no se permiten credenciales predeterminadas. Webpay real, documentos tributarios reales y POS físico requieren contratos, credenciales, certificados y habilitación de los proveedores respectivos.

## Validación

```bash
npm run check
npm run test:commerce-ci-seed
npm run test:commerce-all
```

La suite valida Commerce Core, ventas, pagos, fiscalización, devoluciones, conciliación, clientes, pedidos, reservas, preparación, entrega, concurrencia, seguridad y arranque productivo non-root.

## Rutas principales

- `/` — vitrina y checkout.
- `/admin` — administración y POS.
- `/pedido/<token>` — seguimiento público de pedidos.
- `/health/ready` — readiness productivo.

## Datos de preproducción

Mientras no se ejecute el corte formal de inventario inicial, los productos, precios y existencias utilizados para pruebas se consideran datos de preproducción. El reset y carga física inicial se realizan únicamente mediante el procedimiento de go-live aprobado.

## Regla de aislamiento

La API comercial sólo expone y acepta entidades pertenecientes a **Casa matriz**. Cualquier residuo de otro proyecto debe ser eliminado o permanecer inaccesible hasta su depuración segura; nunca se mezcla con ventas, stock, caja ni clientes de San Pablo.

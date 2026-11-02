# Asistente de pedidos para minimarket

Herramienta que recomienda cuánto pedir de cada producto a partir del historial de ventas y del inventario actual.

**Estado:** en desarrollo (semana 1: definición del problema y de los datos).

## Problema

Hacer los pedidos a proveedores es lento y depende de la intuición: se decide qué y cuánto pedir mirando los estantes. Esto genera dos riesgos: quedarse sin producto (ventas perdidas) o pedir de más (capital inmovilizado y vencimientos).

## Contexto

- Negocio: minimarket de barrio.
- Ventas e inventario se registran en un software de punto de venta (el inventario se descuenta al vender).
- Los pedidos a proveedores se hacen todos los días.

## Objetivo

Generar cada día un listado de pedido sugerido por producto, explicable y fácil de revisar por la persona que decide.

## Alcance de la primera versión

- Entrada: reporte de ventas por producto y por día, e inventario actual.
- Salida: tabla con producto, venta diaria promedio, stock actual y cantidad sugerida a pedir.
- Lógica: Usa una fórmula sencilla y explicable, sin inteligencia artificial.

## Datos necesarios

| Dato | Campos mínimos | Fuente | Estado |
|---|---|---|---|
| Ventas históricas | fecha, producto, cantidad vendida | Reporte del software de punto de venta | Por exportar |
| Inventario actual | producto, cantidad en stock | Software de punto de venta | Por exportar |
| Catálogo de productos | producto, categoría, proveedor | Software de punto de venta | Por exportar |
| Tiempos de entrega | proveedor, días entre pedido y entrega | Definido por la dueña del negocio | Por definir |

## Lógica propuesta (primera versión)

```
venta_diaria_promedio = ventas de los últimos N días / N
cobertura_objetivo    = días que se quiere cubrir (según frecuencia de entrega)
stock_de_seguridad    = margen extra para productos de alta rotación

pedido_sugerido = max(0, venta_diaria_promedio × cobertura_objetivo + stock_de_seguridad − stock_actual)
```

Los valores de N, cobertura y seguridad se calibran con los datos reales.

## Estructura del repositorio

```
/datos        # exportaciones del punto de venta (no subir datos sensibles)
/consultas    # consultas SQL documentadas
/reportes     # salidas generadas
README.md
```

## Próximos pasos

1. Exportar del punto de venta el historial de ventas por producto y por día.
2. Cargar los datos en una base SQL.
3. Escribir y documentar las primeras consultas (venta promedio, rotación, quiebres de stock).
4. Probar la lógica contra pedidos reales pasados.

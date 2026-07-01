/*
===========================================================
Análisis de Desempeño - Nova Retail (2023-2024)

Objetivo:
Identificar los factores que provocaron la disminución de
las ganancias entre 2023 y 2024 mediante un análisis
exploratorio en MySQL.

Herramienta:
MySQL

Autor:
Ashley Abreu
===========================================================
*/

-- ======================================================
-- Análisis financiero anual
-- ======================================================

-- Ganancias por año
SELECT
    YEAR(order_date) AS año,
    SUM(profit) AS ganancias
FROM fact_orders
GROUP BY 1;

-- Ventas por año
SELECT
    YEAR(order_date) AS año,
    SUM(quantity * unit_price) AS ventas
FROM fact_orders
GROUP BY 1;

-- Costos (COGS) por año
SELECT
    YEAR(order_date) AS año,
    SUM(cogs) AS costos
FROM fact_orders
GROUP BY 1;

-- Costos de envío por año
SELECT
    YEAR(order_date) AS año,
    SUM(shipping_cost) AS costo_envio
FROM fact_orders
GROUP BY 1;

-- Margen de ganancia por año
SELECT
    YEAR(order_date) AS año,
    SUM(revenue) AS ingresos,
    SUM(profit) AS ganancias,
    ROUND(SUM(profit)/SUM(revenue)*100,2) AS margen_ganancia_pct
FROM fact_orders
GROUP BY 1;


-- ======================================================
-- Análisis de descuentos
-- ======================================================

-- Descuentos otorgados por año
SELECT
    YEAR(order_date) AS año,
    SUM((quantity * unit_price) * discount_pct) AS descuentos
FROM fact_orders
GROUP BY 1;

-- Descuentos por región (2023)
SELECT
    YEAR(o.order_date) AS año,
    r.region_name,
    SUM((quantity * unit_price) * discount_pct) AS descuentos
FROM fact_orders o
LEFT JOIN dim_regions r
ON r.region_id = o.region_id
GROUP BY 1,2
HAVING año = 2023;

-- Descuentos por región (2024)
SELECT
    YEAR(o.order_date) AS año,
    r.region_name,
    SUM((quantity * unit_price) * discount_pct) AS descuentos
FROM fact_orders o
LEFT JOIN dim_regions r
ON r.region_id = o.region_id
GROUP BY 1,2
HAVING año = 2024;

-- Descuentos por categoría
SELECT
    YEAR(o.order_date) AS año,
    p.category,
    SUM(quantity * unit_price * discount_pct) AS descuentos
FROM fact_orders o
JOIN dim_products p
ON o.product_id = p.product_id
GROUP BY 1,2;

-- Descuentos por subcategoría
SELECT
    YEAR(o.order_date) AS año,
    p.subcategory,
    SUM(quantity * unit_price * discount_pct) AS descuentos
FROM fact_orders o
JOIN dim_products p
ON o.product_id = p.product_id
GROUP BY 1,2;


-- ======================================================
-- Análisis de devoluciones
-- ======================================================

-- Costo de devoluciones por año
SELECT
    YEAR(return_date) AS año,
    SUM(return_cost) AS costo_devoluciones
FROM fact_returns
GROUP BY 1;

-- Cantidad de devoluciones por año
SELECT
    YEAR(return_date) AS año,
    COUNT(return_id) AS cantidad_devoluciones
FROM fact_returns
GROUP BY 1;

-- Razones de devolución (2024)
SELECT
    YEAR(return_date) AS año,
    return_reason,
    COUNT(return_id) AS cantidad_devoluciones
FROM fact_returns
GROUP BY 1,2
HAVING año = 2024;

-- Ventas excluyendo pedidos devueltos
SELECT
    YEAR(o.order_date) AS año,
    SUM(
        CASE
            WHEN r.return_id IS NULL
            THEN o.quantity * o.unit_price
            ELSE 0
        END
    ) AS ventas
FROM fact_orders o
LEFT JOIN fact_returns r
ON o.order_id = r.order_id
GROUP BY 1;

-- Ganancias excluyendo pedidos devueltos
SELECT
    YEAR(o.order_date) AS año,
    SUM(
        CASE
            WHEN r.return_id IS NULL
            THEN o.profit
            ELSE 0
        END
    ) AS ganancias
FROM fact_orders o
LEFT JOIN fact_returns r
ON o.order_id = r.order_id
GROUP BY 1;


-- ======================================================
-- Análisis por categoría
-- ======================================================

-- Ganancias por categoría
SELECT
    YEAR(order_date) AS año,
    p.category,
    SUM(profit) AS ganancias
FROM fact_orders o
JOIN dim_products p
ON o.product_id = p.product_id
GROUP BY 1,2;

-- Margen por categoría
SELECT 
    YEAR(order_date) AS año,
    p.category,
    SUM(o.revenue) AS ingresos,
    SUM(o.profit) AS ganancias,
    ROUND(SUM(o.profit)/SUM(o.revenue)*100,2) AS margen_pct
FROM fact_orders o
JOIN dim_products p
ON o.product_id = p.product_id
GROUP BY 1,2;

-- Costos vs ganancias por categoría
SELECT
    YEAR(order_date) AS año,
    p.category,
    SUM(o.cogs) AS costos,
    SUM(profit) AS ganancias
FROM fact_orders o
LEFT JOIN dim_products p
ON o.product_id = p.product_id
GROUP BY 1,2;

-- Costos de envío por categoría
SELECT
    YEAR(order_date) AS año,
    p.category,
    SUM(quantity * unit_price) AS ventas,
    SUM(shipping_cost) AS costo_envio
FROM fact_orders o
JOIN dim_products p
ON o.product_id = p.product_id
GROUP BY 1,2;


-- ======================================================
-- Análisis de clientes
-- ======================================================

-- Compras por cliente
SELECT
    customer_id,
    SUM(quantity * unit_price) AS ventas
FROM fact_orders
GROUP BY 1;

-- Clientes por segmento
SELECT
    c.segment,
    COUNT(c.customer_id) AS clientes,
    SUM(o.quantity * o.unit_price) AS ventas
FROM fact_orders o
LEFT JOIN dim_customers c
ON o.customer_id = c.customer_id
WHERE YEAR(o.order_date) = 2023
GROUP BY 1;

-- Clientes por canal de adquisición
SELECT
    YEAR(o.order_date) AS año,
    c.acquisition_channel,
    COUNT(c.customer_id) AS clientes,
    SUM(o.quantity * o.unit_price) AS ventas
FROM fact_orders o
LEFT JOIN dim_customers c
ON o.customer_id = c.customer_id
GROUP BY 1,2;


-- ======================================================
-- Análisis por región
-- ======================================================

-- Ganancias por región
SELECT
    YEAR(order_date) AS año,
    r.region_name,
    SUM(profit) AS ganancias
FROM fact_orders o
LEFT JOIN dim_regions r
ON o.region_id = r.region_id
GROUP BY 1,2;


-- ======================================================
-- Observación final
-- ======================================================

-- El análisis sugiere que el incremento en los descuentos
-- estuvo asociado con una disminución significativa de la
-- rentabilidad durante 2024. Además, las categorías
-- Fitness y Accessories presentaron el peor desempeño,
-- registrando márgenes negativos y contribuyendo a la
-- reducción de las ganancias.

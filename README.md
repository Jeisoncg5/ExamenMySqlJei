

# Examen MySQL I 

### Jeison Leonardo Cristancho Garcia

### 



# Modelo Logico



![](https://i.ibb.co/y7jB7sP/Captura-desde-2025-11-07-15-47-52.png)



------



# Codigo SQL base de datos 

```sql
create database ExamenMySql;
use ExamenMySql;

-- Crear tabla de Clientes
CREATE TABLE Clientes (
    idClientes INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(255),
    direccion VARCHAR(255),
    telefono VARCHAR(20)
);
-- Crear tabla de FormaPago
CREATE TABLE FormaPago (
    idFormaPago INT AUTO_INCREMENT PRIMARY KEY,
    nombreFormaPago VARCHAR(255)
);

-- Crear tabla de Adiciones
CREATE TABLE iF NOT exists Adiciones (
    idAdiciones INT AUTO_INCREMENT PRIMARY KEY,
    nombreAdicion VARCHAR(255),
    precioAdicion DECIMAL(10, 2)
);


-- Crear tabla de Ingredientes
CREATE TABLE iF NOT exists Ingredientes (
    idIngredientes INT AUTO_INCREMENT PRIMARY KEY,
    nombreIngrediente VARCHAR(255)
);


-- Crear tabla de Forma_Entrega
CREATE TABLE forma_Entrega (
    idFormaEntrega INT AUTO_INCREMENT PRIMARY KEY,
    nombreFormaEntrega VARCHAR(255)
);
-- Crear tabla de Tipo_Productos
CREATE TABLE tipo_Productos (
    idtipo_Productos INT AUTO_INCREMENT PRIMARY KEY,
    nombreTipoProducto VARCHAR(255)
);
-- Crear tabla de Productos
CREATE TABLE Productos (
    idProductos INT AUTO_INCREMENT PRIMARY KEY,
    nombreProducto VARCHAR(255),
    idtipo_Producto INT,
    precioProducto DECIMAL(10, 2),
    FOREIGN KEY (idtipo_Producto) REFERENCES tipo_Productos(idtipo_Productos)
);

-- Crear tabla de Combos
CREATE TABLE Combos (
    idCombos INT AUTO_INCREMENT PRIMARY KEY,
    nombreCombo VARCHAR(255),
    precioCombo DECIMAL(10, 2)
);

-- Crear tabla de Productos_Combo
CREATE TABLE Productos_Combo (
    idproductos_Combo INT AUTO_INCREMENT PRIMARY KEY,
    idProducto INT,
    idCombo INT,
    precioProductoCombo DECIMAL(10, 2),
    FOREIGN KEY (idProducto) REFERENCES Productos(idProductos),
    FOREIGN KEY (idCombo) REFERENCES Combos(idCombos)
);



-- Crear tabla de Productos_Ingredientes
CREATE TABLE productos_Ingredientes (
    idproductos_Ingredientes INT AUTO_INCREMENT PRIMARY KEY,
    idIngrediente INT,
    idProductos INT,
    FOREIGN KEY (idIngrediente) REFERENCES Ingredientes(idIngredientes),
    FOREIGN KEY (idProductos) REFERENCES Productos(idProductos)
);

-- Crear tabla de Pedidos
CREATE TABLE Pedidos (
    idPedidos INT AUTO_INCREMENT PRIMARY KEY,
    idClientes INT,
    precioPedidoTotal DECIMAL(10, 2),
    idFormaPago INT,
    FOREIGN KEY (idClientes) REFERENCES Clientes(idClientes),
    FOREIGN KEY (idFormaPago) REFERENCES FormaPago(idFormaPago)
);

-- Crear tabla de Pedidos_Productos
CREATE TABLE iF NOT exists pedidos_Productos (
    idpedido_Productos INT AUTO_INCREMENT PRIMARY KEY,
    idProducto INT,
    idPedidos INT,
    idFormaEntrega INT,
    FOREIGN KEY (idPedidos) REFERENCES Pedidos(idPedidos),
    FOREIGN KEY (idFormaEntrega) REFERENCES forma_Entrega(idFormaEntrega)
);

-- Crear tabla de Pedidos_Adiciones
CREATE TABLE iF NOT exists pedidos_Adiciones (
    idpedidos_Adiciones INT AUTO_INCREMENT PRIMARY KEY,
    idAdicion INT,
    idPedido INT,
    FOREIGN KEY (idAdicion) REFERENCES Adiciones(idAdiciones),
    FOREIGN KEY (idPedido) REFERENCES Pedidos(idPedidos)
);

-- Crear tabla de Ingredientes_Adiciones
CREATE TABLE iF NOT exists ingredientes_Adiciones (
    idIngredientes_Adiciones INT AUTO_INCREMENT PRIMARY KEY,
    idIngrediente INT,
    idAdicion INT,
    FOREIGN KEY (idIngrediente) REFERENCES Ingredientes(idIngredientes),
    FOREIGN KEY (idAdicion) REFERENCES Adiciones(idAdiciones)
);


```



# Consultas SQL

**Productos más vendidos (pizza, panzarottis, bebidas, etc.)**

```sql
SELECT p.nombreProducto, COUNT(pp.idProducto) AS cantidad_vendida
FROM Productos p
JOIN pedidos_Productos pp ON p.idProductos = pp.idProducto
```

Select para entrar a la tabla nombreProducto Count para saber el numero de filas para asi saber la cantidad vendida.

**Total de ingresos generados por cada combo**

```sql
SELECT c.nombreCombo, SUM(pc.precioProductoCombo) AS ingresos_generados
FROM Combos c
JOIN Productos_Combo pc ON c.idCombos = pc.idCombo
```

Select para entrar a la tabla nombreCombo Sum para sumar el numero total de columnas y asi saber los ingresos generados por cada combo.

**Pedidos realizados para recoger vs. comer en la pizzería**

```sql
SELECT p.pedidos_Productos,pd.pedidos AS ComerAfuera
FROM pedidos_Productos p
JOIN Pedidos pc ON pd.idPedido = pd.idPedidos
```

**Adiciones más solicitadas en pedidos personalizados**

```sql
SELECT fe.nombreFormaEntrega, COUNT(pp.idpedido_Productos) AS cantidad_pedidos
FROM forma_Entrega fe
JOIN pedidos_Productos pp ON fe.idFormaEntrega = pp.idFormaEntrega
GROUP BY fe.idFormaEntrega;

```



**Cantidad total de productos vendidos por categoría**

```sql
SELECT a.nombreAdicion, COUNT(pa.idpedidos_Adiciones) AS cantidad_solicitada
FROM Adiciones a
JOIN pedidos_Adiciones pa ON a.idAdiciones = pa.idAdicion
GROUP BY a.idAdiciones
ORDER BY cantidad_solicitada DESC;

```



**Promedio de pizzas pedidas por cliente**

```sql
SELECT tp.nombreTipoProducto, COUNT(pp.idproducto_Productos) AS cantidad_vendida
FROM tipo_Productos tp
JOIN Productos p ON tp.idtipo_Productos = p.idtipo_Producto
JOIN pedidos_Productos pp ON p.idProductos = pp.idProducto
GROUP BY tp.idtipo_Productos;

```



**Total de ventas por día de la semana**

```sql
SELECT AVG(cantidad_pedida) AS promedio_pizzas
FROM (
    SELECT idClientes, COUNT(pp.idProducto) AS cantidad_pedida
    FROM Pedidos p
    JOIN pedidos_Productos pp ON p.idPedidos = pp.idPedidos
    JOIN Productos pr ON pp.idProducto = pr.idProductos
    WHERE pr.nombreProducto LIKE '%Pizza%'
    GROUP BY idClientes
) AS subconsulta;

```



**Cantidad de panzarottis vendidos con extra queso**

```sql
SELECT DAYNAME(p.fechaPedido) AS dia_semana, SUM(p.precioPedidoTotal) AS total_ventas
FROM Pedidos p
GROUP BY dia_semana;

```



**Pedidos que incluyen bebidas como parte de un combo**



```sql
SELECT COUNT(pp.idProducto) AS cantidad_vendida
FROM pedidos_Productos pp
JOIN Productos p ON pp.idProducto = p.idProductos
JOIN pedidos_Adiciones pa ON pp.idPedidos = pa.idPedido
WHERE p.nombreProducto LIKE '%Panzarotti%' AND pa.idAdicion = 1; -- Extra Queso

```

**Clientes que han realizado más de 5 pedidos en el último mes**



```sql
SELECT COUNT(DISTINCT pp.idPedidos) AS cantidad_pedidos
FROM pedidos_Productos pp
JOIN Productos p ON pp.idProducto = p.idProductos
WHERE p.nombreProducto LIKE '%Bebida%';

```

**Ingresos totales generados por productos no elaborados (bebidas, postres, etc.)**



```sql
SELECT idClientes, COUNT(idPedidos) AS cantidad_pedidos
FROM Pedidos
WHERE fechaPedido > CURDATE() - INTERVAL 1 MONTH
GROUP BY idClientes
HAVING cantidad_pedidos > 5;

```

**Promedio de adiciones por pedido**

```sql
SELECT SUM(p.precioProducto) AS ingresos_no_elaborados
FROM Productos p
WHERE p.idtipo_Producto IN (3, 4);  -- 3: Bebidas, 4: Postres

```



**Total de combos vendidos en el último mes**

```sql
SELECT AVG(cantidad_adiciones) AS promedio_adiciones
FROM (
    SELECT idPedido, COUNT(idAdicion) AS cantidad_adiciones
    FROM pedidos_Adiciones
    GROUP BY idPedido
) AS subconsulta;

```


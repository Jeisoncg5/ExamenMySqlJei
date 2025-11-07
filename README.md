

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

```



**Pedidos realizados para recoger vs. comer en la pizzería**



```sql

```



**Adiciones más solicitadas en pedidos personalizados**



```sql

```



**Cantidad total de productos vendidos por categoría**



```sql

```



**Promedio de pizzas pedidas por cliente**

```sql

```



**Total de ventas por día de la semana**

```sql

```



**Cantidad de panzarottis vendidos con extra queso**

```sql

```



**Pedidos que incluyen bebidas como parte de un combo**



```sql

```

**Clientes que han realizado más de 5 pedidos en el último mes**



```sql

```

**Ingresos totales generados por productos no elaborados (bebidas, postres, etc.)**



```sql

```

**Promedio de adiciones por pedido**

```sql

```



**Total de combos vendidos en el último mes**

```sql

```



**Clientes con pedidos tanto para recoger como para consumir en el lugar**



```sql

```

**Total de productos personalizados con adiciones**



```sql

```

**Pedidos con más de 3 productos diferentes**

```sql

```



**Promedio de ingresos generados por día**

```sql

```



**Clientes que han pedido pizzas con adiciones en más del 50% de sus pedidos**

```sql

```



**Porcentaje de ventas provenientes de productos no elaborados**



```sql

```

**Día de la semana con mayor número de pedidos para recoger**



```sql

```


## Resuelva todas las consultas utilizando la sintaxis de SQL1 y SQL2. Las consultas con sintaxis de SQL2 se deben resolver con INNER JOIN y NATURAL JOIN.

 1 Obtén un listado con el nombre de cada cliente y el nombre y apellido de su representante de ventas.

```sql
select c.nombre_cliente , e.nombre , e.apellido1 
from cliente c join empleado e on c.codigo_empleado_rep_ventas = e.codigo_empleado 
join pago on codigo_cliente = codigo_cliente ;
```

 2  Muestra el nombre de los clientes que hayan realizado pagos junto con el nombre de sus representantes de ventas.

```sql
select  c.nombre_cliente , e.nombre , e.apellido1 
from cliente c join pago p on c.codigo_cliente = p.codigo_cliente
join empleado e on e.codigo_empleado = c.codigo_empleado_rep_ventas; 
```

 3   Muestra el nombre de los clientes que no hayan realizado pagos junto con el nombre de sus representantes de ventas.

```sql
select c.nombre_cliente, CONCAT(e.nombre, ' ', e.apellido1) as representante
from cliente c
join empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado
where c.codigo_cliente not in (select DISTINCT codigo_cliente from pago);
```

 4 Devuelve el nombre de los clientes que han hecho pagos y el nombre de sus representantes junto con la ciudad de la oficina a la que pertenece el representante.

```sql
select c.nombre_cliente, CONCAT(e.nombre, ' ', e.apellido1) as representante, o.ciudad
from cliente c
join empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado
join oficina o ON e.codigo_oficina = o.codigo_oficina
WHERE c.codigo_cliente IN (select DISTINCT codigo_cliente from pago);
```

 5  Devuelve el nombre de los clientes que no hayan hecho pagos y el nombre de sus representantes junto con la ciudad de la oficina a la que pertenece el representante.

```sql
select c.nombre_cliente, CONCAT(e.nombre, ' ', e.apellido1) AS representante, o.ciudad
from cliente c
join empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado
join oficina o ON e.codigo_oficina = o.codigo_oficina
WHERE c.codigo_cliente NOT IN (select DISTINCT codigo_cliente from pago);
```

 6 Lista la dirección de las oficinas que tengan clientes en Fuenlabrada.

```sql
select  DISTINCT concat(' principal ', o.linea_direccion1,' complemento ', o.linea_direccion2) as direccion_oficina 
from oficina o join empleado e on o.codigo_oficina = e.codigo_oficina 
join cliente c on e.codigo_empleado = c.codigo_empleado_rep_ventas where c.ciudad = 'Fuenlabrada';
```

 7 Devuelve el nombre de los clientes y el nombre de sus representantes junto con la ciudad de la oficina a la que pertenece el representante.

```sql
select c.nombre_cliente, concat(e.nombre, ' ', e.apellido1, ' ', e.apellido2) as nombre_representante, o.ciudad  from 
cliente c join empleado e on c.codigo_empleado_rep_ventas = e.codigo_empleado
join oficina o on e.codigo_oficina = o.codigo_oficina;
```

 8  Devuelve un listado con el nombre de los empleados junto con el nombre de sus jefes.

```sql
select concat(e1.nombre, ' ', e1.apellido1 ,' ' , e1.apellido2)  as empleado,e1.codigo_jefe , e2.codigo_empleado,
 concat(e2.nombre, ' ', e2.apellido1 ,' ' , e2.apellido2)as jefe from 
 empleado e1 join empleado e2 on e1.codigo_jefe = e2.codigo_empleado;
 ```

 9 Devuelve un listado que muestre el nombre de cada empleados, el nombre de su jefe y el nombre del jefe de sus jefe.

```sql
select concat(e1.nombre, ' ', e1.apellido1 ,' ' , e1.apellido2)  as empleado,
concat(e2.nombre, ' ', e2.apellido1 ,' ' , e2.apellido2) as jefe ,
concat(e3.nombre, ' ', e3.apellido1 ,' ' , e3.apellido2) as jefe_de_jefes  
from 
empleado e1 join empleado e2 on e1.codigo_jefe = e2.codigo_empleado
join empleado e3 on e2.codigo_jefe = e3.codigo_empleado order by empleado asc; 
 ```

 10  Devuelve el nombre de los clientes a los que no se les ha entregado a tiempo un pedido.

```sql
select c.nombre_cliente as cliente, p.fecha_esperada, p.fecha_entrega ,p.estado  from
cliente c join pedido p on c.codigo_cliente = p.codigo_cliente
where p.fecha_entrega is null or estado = 'Pendiente'; 
```

  11.   Devuelve un listado de las diferentes gamas de producto que ha comprado cada cliente.

```sql
select  c.codigo_cliente , c.nombre_cliente, group_concat(DISTINCT pr.gama separator ', ' ) as gama_producto  from cliente c 
join pedido p on c.codigo_cliente = p.codigo_cliente 
join detalle_pedido dp on p.codigo_pedido =  dp.codigo_pedido 
join producto pr on dp.codigo_producto = pr.codigo_producto 
GROUP BY codigo_cliente
order by codigo_cliente asc;
 ```

## Resuelva todas las consultas utilizando las cláusulas LEFT JOIN, RIGHT JOIN, NATURAL LEFT JOIN y NATURAL RIGHT JOIN.

 1  Devuelve un listado que muestre solamente los clientes que no han realizado ningún pago.

```sql
select c.nombre_cliente, p.codigo_cliente from cliente c 
left join pago p on c.codigo_cliente = p.codigo_cliente where p.codigo_cliente is null;
```

 2 Devuelve un listado que muestre solamente los clientes que no han realizado ningún pedido.

```sql
select c.nombre_cliente, p.codigo_cliente from cliente c
left join pedido p on c.codigo_cliente = p.codigo_cliente where p.codigo_cliente is null;
```

 3 Devuelve un listado que muestre los clientes que no han realizado ningún pago y los que no han realizado ningún pedido.

```sql
select c.codigo_cliente, c.nombre_cliente, pa.codigo_cliente , pe.codigo_cliente from cliente c
left join pedido pe on c.codigo_cliente = pe.codigo_cliente
left join pago pa on c.codigo_cliente = pa.codigo_cliente
where pa.codigo_cliente is null and pe.codigo_cliente is null;
```


 4 Devuelve un listado que muestre solamente los empleados que no tienen una oficina asociada.

```sql
select concat(e.nombre, ' ', e.apellido1 ,' ' , e.apellido2)  as empleado, 
o.codigo_oficina from empleado e 
left join oficina o on e.codigo_oficina = o.codigo_oficina
where o.codigo_oficina is null;
```

 5 Devuelve un listado que muestre solamente los empleados que no tienen un cliente asociado.

```sql
select concat(e.nombre, ' ', e.apellido1 ,' ' , e.apellido2)  as empleado, 
c.codigo_cliente from empleado e 
left join cliente c on e.codigo_empleado = c.codigo_empleado_rep_ventas 
where c.codigo_cliente is null ;
```

 6 Devuelve un listado que muestre solamente los empleados que no tienen un cliente asociado junto con los datos de la oficina donde trabajan.

```sql
select concat(e.nombre, ' ', e.apellido1 ,' ' , e.apellido2)  as empleado, 
c.codigo_cliente,e.codigo_oficina, o.ciudad 
from empleado e 
left join cliente c on e.codigo_empleado = c.codigo_empleado_rep_ventas
left join oficina o on e.codigo_oficina = o.codigo_oficina 
where c.codigo_cliente is null ;
```


 7 Devuelve un listado que muestre los empleados que no tienen una oficina asociada y los que no tienen un cliente asociado.

```sql
select concat(e.nombre, ' ', e.apellido1 ,' ' , e.apellido2)  as empleado,  
c.codigo_cliente , o.codigo_oficina  from empleado e 
left join cliente c on e.codigo_empleado = c.codigo_empleado_rep_ventas
left join oficina o on e.codigo_oficina = o.codigo_oficina
where o.codigo_oficina is null or c.codigo_cliente is null 
order by e.nombre;
```


 8 Devuelve un listado de los productos que nunca han aparecido en un pedido.

```sql
select p.nombre ,dp.codigo_producto
from producto p 
left join detalle_pedido dp on p.codigo_producto = dp.codigo_producto
where dp.codigo_producto is null;
```


 9 Devuelve un listado de los productos que nunca han aparecido en un pedido. El resultado debe mostrar el nombre, la descripción y la imagen del producto.

```sql
select distinct p.nombre , p.descripcion, gp.imagen ,dp.codigo_producto
from producto p
left join detalle_pedido dp on p.codigo_producto = dp.codigo_producto
left join gama_producto gp on p.gama = gp.gama 
where dp.codigo_producto is null;
```


 10 Devuelve las oficinas donde no trabajan ninguno de los empleados que hayan sido los representantes de ventas de algún cliente que haya realizado la compra de algún producto de la gama Frutales.

```sql
select distinct o.* 
from oficina o
join empleado e on o.codigo_oficina = e.codigo_oficina
join cliente c on e.codigo_empleado = c.codigo_empleado_rep_ventas
join pedido p on c.codigo_cliente = p.codigo_cliente
join detalle_pedido dp on p.codigo_pedido = dp.codigo_pedido
join producto pr on dp.codigo_producto = pr.codigo_producto
where pr.nombre != 'Frutales';
```

 11 Devuelve un listado con los clientes que han realizado algún pedido pero no han realizado ningún pago.

```sql
select distinct c.*
from cliente c
join pedido pe on c.codigo_cliente = pe.codigo_cliente
left join pago pa on c.codigo_cliente = pa.codigo_cliente
where pa.codigo_cliente is null;
```

 12 Devuelve un listado con los datos de los empleados que no tienen clientes asociados y el nombre de su jefe asociado.

```sql
select distinct e1.* , concat(e2.nombre, ' ', e2.apellido1 ,' ' , e2.apellido2) as jefe 
from empleado e1 
left join cliente c on e1.codigo_empleado = c.codigo_empleado_rep_ventas
left join empleado e2 on e1.codigo_jefe = e2.codigo_empleado 
where  c.codigo_empleado_rep_ventas is null
order by e1.nombre;
```
1. Devuelve un listado con el código de oficina y la ciudad donde hay oficinas.
```SQL
select codigo_oficina, ciudad from oficina;
```

2. Devuelve un listado con la ciudad y el teléfono de las oficinas de España.
```SQL
select ciudad, telefono from oficina where pais = 'españa';
```

3. Devuelve un listado con el nombre, apellidos y email de los empleados cuyo jefe tiene un código de jefe igual a 7.
```SQL
select nombre, apellido1, apellido2, email from empleado where codigo_jefe = 7;
```

4. Devuelve el nombre del puesto, nombre, apellidos y email del jefe de la empresa.
```SQL
select puesto, nombre, apellido1, apellido2, email from empleado where codigo_jefe is null;
```

5. Devuelve un listado con el nombre, apellidos y puesto de aquellos empleados que no sean representantes de ventas.
```SQL
select nombre, apellido1, apellido2, puesto from empleado where puesto <> 'representante de ventas';
```
6. Devuelve un listado con el nombre de los todos los clientes españoles.
```SQL
select nombre_cliente from cliente where pais = 'spain';
```

7. Devuelve un listado con los distintos estados por los que puede pasar un pedido.
```SQL
select distinct estado from pedido;
```

8. Devuelve un listado con el código de cliente de aquellos clientes que realizaron algún pago en 2008. Tenga en cuenta que deberá eliminar aquellos códigos de cliente que aparezcan repetidos. Resuelva la consulta:

Utilizando la función YEAR de MySQL.

- Utilizando la función DATE_FORMAT de MySQL.
- Sin utilizar ninguna de las funciones anteriores.

```SQL
select distinct codigo_cliente from pago where year(fecha_pago) = 2008;
```

```SQL
select distinct codigo_cliente from pago where date_format(fecha_pago, '%Y') = '2008';
```

```SQL
select distinct codigo_cliente from pago where fecha_pago >= '2008-01-01' and fecha_pago < '2009-01-01';
```

9. Devuelve un listado con el código de pedido, código de cliente, fecha esperada y fecha de entrega de los pedidos que no han sido entregados a tiempo.
```SQL
select codigo_pedido, codigo_cliente, fecha_esperada, fecha_entrega from pedido where fecha_entrega > fecha_esperada;

```
10. Devuelve un listado con el código de pedido, código de cliente, fecha esperada y fecha de entrega de los pedidos cuya fecha de entrega ha sido al menos dos días antes de la fecha esperada.

Utilizando la función ADDDATE de MySQL.

- Utilizando la función DATEDIFF de MySQL.
- ¿Sería posible resolver esta consulta utilizando el operador de suma + o resta - ?
<!-- add date -->
```SQL
select codigo_pedido, codigo_cliente, fecha_esperada, fecha_entrega
from pedido
where fecha_entrega <= ADDDATE(fecha_esperada, -2);
```
<!-- DATEDIFF  -->
```SQL
select codigo_pedido, codigo_cliente, fecha_esperada, fecha_entrega
from pedido
where DATEDIFF(fecha_entrega, fecha_esperada) <= -2;

```
<!-- suma + o resta - -->
```SQL
select codigo_pedido, codigo_cliente, fecha_esperada, fecha_entrega
from pedido
where fecha_entrega <= fecha_esperada - INTERVAL 2 DAY;
```
11. Devuelve un listado de todos los pedidos que fueron rechazados en 2009.
```SQL
select *
from pedido
where estado = 'rechazado' and YEAR(fecha_pedido) = 2009;
```

12. Devuelve un listado de todos los pedidos que han sido entregados en el mes de enero de cualquier año.
```SQL
select *
from pedido
where MONTH(fecha_entrega) = 1;
```

13. Devuelve un listado con todos los pagos que se realizaron en el año 2008 mediante Paypal. Ordene el resultado de mayor a menor.
```SQL
select *
from pago
where YEAR(fecha_pago) = 2008 and forma_pago = 'Paypal'
order by total DESC;
```

14. Devuelve un listado con todas las formas de pago que aparecen en la tabla pago. Tenga en cuenta que no deben aparecer formas de pago repetidas.
```SQL
select DISTINCT forma_pago
from pago;
```

15. Devuelve un listado con todos los productos que pertenecen a la gama Ornamentales y que tienen más de 100 unidades en stock. El listado deberá estar ordenado por su precio de venta, mostrando en primer lugar los de mayor precio.
```SQL
select *
from producto
where gama = 'Ornamentales' and cantidad_en_stock > 100
order by precio_venta DESC;
```

16. Devuelve un listado con todos los clientes que sean de la ciudad de Madrid y cuyo representante de ventas tenga el código de empleado 11 o 30.
```SQL
select *
from cliente
where ciudad = 'Madrid' and (codigo_empleado_rep_ventas = 11 or codigo_empleado_rep_ventas = 30);
```
### 1.4.8 Subconsultas

#### 1.4.8.1 Con operadores básicos de comparación

1. Devuelve el nombre del cliente con mayor límite de crédito.
```sql
select * 
from cliente
where limite_credito = (
select max(limite_credito) 
from cliente);  
```

2. Devuelve el nombre del producto que tenga el precio de venta más caro.
```sql
select nombre
from producto
where precio_venta = (select max(precio_venta) from producto);
```
3. Devuelve el nombre del producto del que se han vendido más unidades. (Tenga en cuenta que tendrá que calcular cuál es el número total de unidades que se han vendido de cada producto a partir de los datos de la tabla `detalle_pedido`)
```sql
select nombre
from producto
where codigo_producto = (
    select codigo_producto
    from detalle_pedido
    group by codigo_producto
    order by sum(cantidad) desc
    limit 1
);

```


4. Los clientes cuyo límite de crédito sea mayor que los pagos que haya realizado. (Sin utilizar `INNER JOIN`).
```sql
select nombre_cliente
from cliente
where limite_credito > (
    select sum(total)
    from pago
    where pago.codigo_cliente = cliente.codigo_cliente
    group by codigo_cliente
);

```

5. Devuelve el producto que más unidades tiene en stock.
```sql
select nombre, cantidad_en_stock
from producto
order by cantidad_en_stock desc
limit 1;
```
```sql
select p.* 
from producto p 
where p.cantidad_en_stock = (
  select max(p.cantidad_en_stock)
  from producto p 
)
order by p.nombre;
```

6. Devuelve el producto que menos unidades tiene en stock.
```sql 
select nombre
from producto
where cantidad_en_stock = (
    select min(cantidad_en_stock)
    from producto
);

```

7. Devuelve el nombre, los apellidos y el email de los empleados que están a cargo de **Alberto Soria**.
```sql
select e.nombre, e.apellido1, e.email
from empleado e
where e.codigo_jefe = (
    select e2.codigo_empleado
    from empleado e2
    where concat(e2.nombre, ' ', e2.apellido1) = 'Alberto Soria'
);

```
#### 1.4.8.2 Subconsultas con ALL y ANY


1. Devuelve el nombre del cliente con mayor límite de crédito.
```sql 
select nombre_cliente,limite_credito
from cliente
where limite_credito = (select max(limite_credito) from cliente);
```

2. Devuelve el nombre del producto que tenga el precio de venta más caro.
```sql 
select nombre
from producto
where precio_venta = (select max(precio_venta) from producto);
```

3. Devuelve el producto que menos unidades tiene en stock.
```sql 
select nombre
from producto
where cantidad_en_stock = (select min(cantidad_en_stock) from producto);
```

#### 1.4.8.3 Subconsultas con IN y NOT IN


1. Devuelve el nombre, apellido1 y cargo de los empleados que no representen a ningún cliente.
```sql 
select nombre, apellido1, puesto
from empleado
where codigo_empleado not in (select distinct codigo_empleado_rep_ventas from cliente);
```

2. Devuelve un listado que muestre solamente los clientes que no han realizado ningún pago.
```sql 
select *
from cliente
where codigo_cliente not in (select distinct codigo_cliente from pago);
```

3. Devuelve un listado que muestre solamente los clientes que sí han realizado algún pago.
```sql 
select *
from cliente
where codigo_cliente in (select distinct codigo_cliente from pago);
```

4. Devuelve un listado de los productos que nunca han aparecido en un pedido.
```sql 
select *
from producto
where codigo_producto not in (select distinct codigo_producto from detalle_pedido);
```

5. Devuelve el nombre, apellidos, puesto y teléfono de la oficina de aquellos empleados que no sean representante de ventas de ningún cliente.
```sql 
select e.nombre, e.apellido1, e.puesto, o.telefono
from empleado e
join oficina o on e.codigo_oficina = o.codigo_oficina
where e.codigo_empleado not in (select distinct codigo_empleado_rep_ventas from cliente);
```

6. Devuelve las oficinas donde **no trabajan** ninguno de los empleados que hayan sido los representantes de ventas de algún cliente que haya realizado la 
compra de algún producto de la gama `Frutales`.

```sql 
select distinct o.*
from oficina o
left join empleado e on o.codigo_oficina = e.codigo_oficina
where e.codigo_empleado not in (select distinct codigo_empleado_rep_ventas from cliente where codigo_cliente in (select distinct codigo_cliente from pedido p join detalle_pedido dp on p.codigo_pedido = dp.codigo_pedido join producto pr on dp.codigo_producto = pr.codigo_producto where pr.gama = 'Frutales'));
```
7. Devuelve un listado con los clientes que han realizado algún pedido pero no han realizado ningún pago.

```sql
select *
from cliente
where codigo_cliente in (select distinct codigo_cliente from pedido) and codigo_cliente not in (select distinct codigo_cliente from pago);
 ```

#### 1.4.8.4 Subconsultas con EXISTS y NOT EXISTS


1. Devuelve un listado que muestre solamente los clientes que no han realizado ningún pago.
```sql 
select *
from cliente c
where not exists (select 1 from pago p where c.codigo_cliente = p.codigo_cliente);
```

2. Devuelve un listado que muestre solamente los clientes que sí han realizado algún pago.
```sql 
select *
from cliente c
where exists (select 1 from pago p where c.codigo_cliente = p.codigo_cliente);
```

3. Devuelve un listado de los productos que nunca han aparecido en un pedido.
```sql 
select *
from producto pr
where not exists (select 1 from detalle_pedido dp where pr.codigo_producto = dp.codigo_producto);
```

4. Devuelve un listado de los productos que han aparecido en un pedido alguna vez.
```sql 
select  *
from producto pr
where exists (select 1 from detalle_pedido dp where pr.codigo_producto = dp.codigo_producto);
```

## 5 TIPS con GROUP BY 

1. muestre el nombre, limite de credito y cantidad de pedidos que a hecho cada cliente
```sql 
select c.nombre_cliente , c.limite_credito, count(*) as total_pedido_por_cliente 
from cliente c
join pedido p on c.codigo_cliente = p.codigo_cliente
group by c.nombre_cliente , c.limite_credito;  
```
2. Mostrar el total de ventas por cliente que tenga al menos dos pedidos realizados.

```sql 
select nombre_cliente , total_ventas
from (
  select c.nombre_cliente, p.codigo_pedido , sum(dp.cantidad * dp.precio_unidad ) as total_ventas
  from cliente c
  join pedido p on c.codigo_cliente = p.codigo_cliente
  join detalle_pedido dp on p.codigo_pedido = dp.codigo_pedido
  group by c.nombre_cliente , p.codigo_pedido
) as ventas_por_pedido
group by nombre_cliente
having count(codigo_pedido) >= 2 ;

```
<!-- funciones escalares  substring --> 
3. muestra los primeros 30 caracteres de la descripción de los productos de la tabla "producto".

```sql 
select codigo_producto, nombre, SUBSTRING(descripcion, 1, 30) as descripcion_cortada
FROM producto;
```
```sql 
select nombre , left(descripcion, 30) as descripcion_cortada from producto;
```
<!-- agrupado con union all --> 
4. combina y muestra los nombres de clientes de la tabla "cliente" con los nombres de empleados de la tabla "empleado" en una lista única utilizando UNION ALL
```sql 
select nombre_cliente as nombre , 
'cliente' as tipo from cliente 
union all 
select concat(nombre ,' ',apellido1,' ') as nombre ,
'empleado' as tipo 
from empleado;
```
<!-- concatenar los valores de un resultado en una selda  , group_concat --> 
5. Concatena los nombre de empleados con su cliente asignado

```sql 
select e.nombre as nombre_empleado ,
group_concat(c.nombre_cliente) as nombre_cliente
from empleado e
left join cliente c on e.codigo_empleado = c.codigo_empleado_rep_ventas
group by e.nombre;
```
<!-- totales por cada agrupamiento , group by rollup ,ifnull--> 
6. Obtén un informe que muestre el total de ventas por gama de producto y, adicionalmente, el gran total de todas las ventas. Si una gama no tiene ventas, se debe mostrar como "Sin ventas".

```sql 
select ifnull(g.gama,'total') as gama,
ifnull(sum(p.precio_venta),'sin ventas') as total_ventas
from gama_producto g 
left join producto p on g.gama = p.gama
group by g.gama with rollup;
```

## 5 TIPS con WHERE en SQL

1.  muestra los clientes que no han realizado ningún pedido.

```sql 
select nombre_cliente
from cliente
where codigo_cliente not in (select distinct codigo_cliente from pedido);
```

2. obten el nombre de los clientes que han realizado pedidos más de una vez.

```sql 
select c.nombre_cliente
from cliente c
where c.codigo_cliente in (
    select distinct codigo_cliente
    from pedido
    group by codigo_cliente
    having count(codigo_pedido) > 1
);
```
3. obten una lista de empleados cuyos nombres comiencen con 'M' y cuyo apellido1  comience con 'p'.

```sql 
select *
from empleado
where nombre regexp '^m' and apellido1 regexp '^p';
```

4. obten una lista de empleados que trabajan en oficinas ubicadas en las ciudades de Barcelona y Madrid
```sql 
select *
from empleado
where codigo_oficina in (
    select distinct codigo_oficina
    from oficina
    where ciudad in ('Barcelona', 'Madrid')
);
```

5. convierte los nombres de los empleados a mayusculas
```sql 
SELECT UPPER(nombre) AS nombre_en_mayusculas
FROM empleado;
```

## TIPS con UPDATE en SQL

editar utilizando el valor ya guardado

1. Actualiza el correo electrónico del empleado con el código 5. concatenando su primer apellido, la extensión actual del empleado con el dominio "@jardineria.es".

```sql 
update empleado
set email = CONCAT(apellido1, extension, '@jardineria.es')
where codigo_empleado = 5;
```

multiple agrupamiento 

2. Aumenta en un 5% el precio de venta de todos los productos en la tabla de productos que pertenecen a gamas con al menos 3 productos distintos.
```sql 
update producto
set precio_venta = precio_venta * 1.05
where gama IN (
    select gama
    from producto
    group by gama
    having COUNT(distinct codigo_producto) >= 3
);

```
editar obteinedo e valor por subconsulta 

3. Aumenta en un 10% el límite de crédito de todos los clientes cuyo representante de ventas tiene el código 22. Esta actualización se realiza en función de la relación con el empleado que tiene el código especificado.
```sql 
update cliente
set limite_credito = limite_credito * 1.1
where codigo_empleado_rep_ventas in (
  select codigo_empleado 
  from empleado
  where codigo_empleado = 22
);
```

4. muestre todos los clientes que tienen como representante de ventas al empleado con el codigo 22 mediante subconsultas
```sql 
select * 
from cliente 
where codigo_empleado_rep_ventas in (
  select codigo_empleado 
  from empleado
  where codigo_empleado =22
);
```

subconsultas en where 

5. Muestra todos los clientes cuyos representantes de ventas tienen el puesto "Director Oficina".

```sql 
select * 
from cliente 
where codigo_empleado_rep_ventas in (
  select codigo_empleado
  from empleado
  where puesto =  'Director Oficina'
);
```

update con join 

6.  Actualiza los precios unitarios en los detalles de los pedidos para que coincidan con los nuevos precios de venta de los productos.

```sql 
update detalle_pedido dp
join producto p on dp.codigo_producto = p.codigo_producto
set dp.precio_unidad  = p.precio_venta 
where dp.precio_unidad < p.precio_venta;
```

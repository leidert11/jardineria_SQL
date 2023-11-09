## Resuelva todas las consultas utilizando la sintaxis de SQL1 y SQL2. Las consultas con sintaxis de SQL2 se deben resolver con INNER JOIN y NATURAL JOIN.

 1 Obtén un listado con el nombre de cada cliente y el nombre y apellido de su representante de ventas.

```sql
select c.nombre_cliente , e.nombre , e.apellido1 
from cliente c join empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado 
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
select c.nombre_cliente, CONCAT(e.nombre, ' ', e.apellido1) AS representante
from cliente c
join empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado
WHERE c.codigo_cliente NOT IN (select DISTINCT codigo_cliente from pago);
```

 4 Devuelve el nombre de los clientes que han hecho pagos y el nombre de sus representantes junto con la ciudad de la oficina a la que pertenece el representante.

```sql
select c.nombre_cliente, CONCAT(e.nombre, ' ', e.apellido1) AS representante, o.ciudad
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
 concat(e3.nombre, ' ', e3.apellido1 ,' ' , e3.apellido2) as jefe_de_jefes  from 
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

```


 8 Devuelve un listado de los productos que nunca han aparecido en un pedido.

```sql

```


 9 Devuelve un listado de los productos que nunca han aparecido en un pedido. El resultado debe mostrar el nombre, la descripción y la imagen del producto.

```sql

```


 10 Devuelve las oficinas donde no trabajan ninguno de los empleados que hayan sido los representantes de ventas de algún cliente que haya realizado la compra de algún producto de la gama Frutales.

```sql

```


 11 Devuelve un listado con los clientes que han realizado algún pedido pero no han realizado ningún pago.

```sql

```


 12 Devuelve un listado con los datos de los empleados que no tienen clientes asociados y el nombre de su jefe asociado.

```sql

```

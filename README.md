# Caso_Practico_SQL_Sabores_del_Mundo
El restaurante "Sabores del Mundo", es conocido por su auténtica cocina y su ambiente  acogedor.  Este restaurante lanzó un nuevo menú a principios de año y ha estado recopilando  información detallada sobre las transacciones de los clientes para identificar áreas de  oportunidad y aprovechar al máximo sus datos para optimizar las ventas.

## b) Explorar la tabla “menu_items” para conocer los productos del menú.
  ### 1.- Realizar consultas para contestar las siguientes preguntas:
  
  ● Encontrar el número de artículos en el menú.
      R = 32 diferentes articulos
      
         select count(distinct(item_name)) from menu_items;

  ● ¿Cuál es el artículo menos caro y el más caro en el menú? 
      R = Shimp Scampi con 19.95 como mas caro , Edamame con 5 como el mas barato
      
       select item_name, price
       from menu_items
       order by price desc
       limit 1;
      
       select item_name, price
       from menu_items
       order by price asc
       limit 1;
       
  ● ¿Cuántos platos americanos hay en el menú?
      R= 6 platos de categoria americana
  
        select count(category) from menu_items
        where category = 'American';

  ● ¿Cuál es el precio promedio de los platos? 
      R = 13.29 es el promedio promedio de todos los platillos
        
        select round(avg(price),2) as precio_promedio_platillo from menu_items;

## c) Explorar la tabla “order_details” para conocer los datos que han sido recolectados.
  ### 1.- Realizar consultas para contestar las siguientes preguntas:

   ● ¿Cuántos pedidos únicos se realizaron en total?
      R = Se realizaron un total de 5370 ordenes distintas
       
        select * from order_details;
        select count(distinct(order_id)) from order_details;
        
   ● ¿Cuáles son los 5 pedidos que tuvieron el mayor número de artículos?
      R = 116, 195, 23, 70, 245 son los que tuvieron mayor número de artículos entre otros.
        
        select order_details_id, item_id
        from order_details
        where item_id is not null
        order by item_id desc
        limit 5

   ● ¿Cuándo se realizó el primer pedido y el último pedido?
       R = El último pedido fue el 2023-03-31 y el primer pedido fue el 2023-01-01
       
          Select order_date from order_details
          order by order_date asc
          limit 1;
        
          Select order_date from order_details
         order by order_date desc
         limit 1;

   ● ¿Cuántos pedidos se hicieron entre el '2023-01-01' y el '2023-01-05'?
      R = se hicieron 702 pedidos durante el periodo '2023-01-01' y el '2023-01-05'

        SELECT COUNT(order_details_id)
        FROM order_details
        WHERE order_date >= '2023-01-01' and order_date <= '2023-01-05' ;

## d) Usar ambas tablas para conocer la reacción de los clientes respecto al menú.
  ### 1.- Realizar un left join entre entre order_details y menu_items con el identificador item_id(tabla order_details) y menu_item_id(tabla menu_items).

         select * from menu_items as m
         left join order_details as o
         on m.menu_item_id = o.item_id

## e) Una vez que hayas explorado los datos en las tablas correspondientes y respondido las preguntas planteadas, realiza un análisis adicional utilizando este join entre las tablas. El objetivo es identificar 5 puntos clave que puedan ser de utilidad para los dueños del restaurante en el lanzamiento de su nuevo menú. Para ello, crea tus propias consultas y utiliza los resultados obtenidos para llegar a estas conclusiones.

   ●  Cuál es el top 5 de pedidos mexicana mas vendidos?
        
        select item_name, count(order_id) as total_orders 
         from menu_items as m
         left join order_details as o
         on m.menu_item_id = o.item_id
         where m.category = 'Mexican'
         group by m.item_name
         order by total_orders desc
         limit 5;

  ●  Cuál es la suma de precios por categoria?
     
      select category, sum(price) as suma_precios
       from menu_items as m 
       left join order_details as o 
       on m.menu_item_id = o.item_id
       group by m.category
       order by suma_precios desc;
       
  ●  En que fecha se gano mas al vender hamburguesas?
    
      select item_name, order_date, sum(price) as suma_precios
     from menu_items as m 
     left join order_details as o 
     on m.menu_item_id = o.item_id
     where m.item_name = 'Hamburger'
     group by item_name, o.order_date
     order by suma_precios desc
     limit 1;

  ●  En que fecha se vendieron menos French Fries
     
      select item_name, order_date, sum(price) as suma_precios
     from menu_items as m 
     left join order_details as o 
     on m.menu_item_id = o.item_id
     where m.item_name = 'French Fries'
     group by item_name, o.order_date
     order by suma_precios asc
     limit 1;

  ● Cuales son los productos mas vendidos
    
       select item_name, count(order_id) as total_orders 
     from menu_items as m
     left join order_details as o
     on m.menu_item_id = o.item_id
     group by m.item_name
     order by total_orders desc
     limit 5;






        

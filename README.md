# ActividadDecimasCloud

# 🐳 Configuración de WordPress con MariaDB usando Docker

Primero definimos los servicios, los cuales serán **WordPress** y la base de datos con **MariaDB**.  

---
services: 

  wordpress: --> nombre del servicio  

    image: wordpress:6-apache --> usa la imagen de WordPress versión 6, basada en Apache  

    container_name: wp --> nombre del contenedor  

    ports:  
      - "8082:80" --> expone el puerto 80 del contenedor (donde corre Apache) al 8082 del host  

    environment: --> las variables de entorno para que WordPress se conecte a la base de datos  
    definimos la dirección del contenedor de BD y su puerto, un usuario a la BD, una contraseña del usuario y el nombre de la BD que usará WordPress  

      WORDPRESS_DB_HOST: db:3306  
      WORDPRESS_DB_USER: wp_user  
      WORDPRESS_DB_PASSWORD: wp_pass  
      WORDPRESS_DB_NAME: wordpress  

    volumes: --> crea un volumen persistente (wp_data) para guardar los archivos de WordPress  
      - wp_data:/var/www/html  

    restart: unless-stopped --> el contenedor se reiniciará automáticamente a menos que se detenga manualmente  

---

  db: --> nombre del servicio para la base de datos  

    image: mariadb:11 --> usa la imagen oficial de MariaDB versión 11  

    container_name: wpdb --> nombre del contenedor de la BD  

    environment: --> las variables de entorno para iniciar la BD  
    definimos la contraseña del usuario root de MariaDB, crea una BD llamada WordPress, crea un usuario y una contraseña para este usuario  

      MYSQL_ROOT_PASSWORD: rootpass  
      MYSQL_DATABASE: wordpress  
      MYSQL_USER: wp_user  
      MYSQL_PASSWORD: wp_pass  

    volumes: --> crea un volumen persistente para almacenar los datos de MariaDB  
      - wp_db_data:/var/lib/mysql  

    restart: unless-stopped --> el contenedor se reiniciará automáticamente a menos que se detenga manualmente  

---

volumes: --> define los volúmenes persistentes utilizados por ambos servicios  

  wp_data:  
  wp_db_data:  
  

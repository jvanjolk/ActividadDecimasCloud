# ActividadDecimasCloud

# 🐳 Configuración de WordPress con MariaDB usando Docker
Primero definimos los servicios, los cuales seran WordPress y la base de datos con MariaDB

**services:**
  **wordpress:** --> nombre del servicio
    image: wordpress:6-apache --> usa la imagen de WordPress version 6, basada en Apache
    container_name: wp --> nombre del contenedor 
    ports:
      - "8082:80" --> expone el puerto 80 del contenedor (donde corre Apache) al 8082 del host
    environment: --> las variables de entorno para que wordpress se conecte a la base de datos
    definimos la direccion del contenedor de bd y su puerto, un usuario a la bd, una contraseña del usuario y el nombre de la bd que usara wordpress
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wp_user
      WORDPRESS_DB_PASSWORD: wp_pass
      WORDPRESS_DB_NAME: wordpress
    volumes: --> crea un volumen persistente (wp_data) para guardar los archivos de wordpress
      - wp_data:/var/www/html
    restart: unless-stopped --> el contenedor se reiniciará automáticamente a menos que se detenga manualmente

  db: --> nombre del servicio para la base de datos
    image: mariadb:11 --> usa la imagen oficial de MariaDB version 11
    container_name: wpdb --> nombre del contenedor de la bd
    environment: --> las variables de entorno para iniciar la bd
    definimos la contraseña del usuario root de mariadb, crea una bd llamada wordpress, crea un usuario y una contraseña para este usuario
      MYSQL_ROOT_PASSWORD: rootpass
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wp_user
      MYSQL_PASSWORD: wp_pass
    volumes: -->  crea un volumen persistente para almacenar los datos de mariadb
      - wp_db_data:/var/lib/mysql
    restart: unless-stopped --> el contenedor se reiniciará automáticamente a menos que se detenga manualmente

volumes: --> define los volúmenes persistentes utilizados por ambos servicios
  wp_data:
  wp_db_data:

  

# Creacion/Fortificación de una web Wordpress

En esta práctica desplegaremos un servidor web local utilizando el CMS Wordpress. La web tendrá la temática de la pescadería
"Mariscos-Recio" de la serie española "La que se avecina".

Todo se realizará en un entorno Linux(Ubuntu) y por comandos de consola (para mi opinión, mas sencillo que en Windows)

Lo primero es instalar los programas necesitados por wordpress. Tanto apache2 que genera un servidor como mariadb para la base
de datos.
```console
$: sudo apt install mariadb-server
$: sudo apt install apache2
```
Antes de empezar con el servidor hay que preparar la base de datos para que la utilize www-data.

WWW-DATA es el demonio (usaurio del sistema automatico) que correrá el servidor web. La carpeta en la que operará es 
/var/www/html que es donde estarán todos los archivos de la web.

```console
$: sudo service mysql start
$: sudo mysql -uroot
Mariadb[(None)]> create database wordpress;
Mariadb[(None)]> use wordpress;
Mariadb(wordpress)> create user 'wp-admin'@'localhost' identified by 'antonio-recio';
Mariadb(wordpress)> grant all privileges on wordpress.* to 'wp-admin'@'localhost';
```

Descargamos el wordpress de la web oficial y lo unzipeamos. Luego lo metemos en /var/www/html.

```console
$:~/Downloads unzip wordpress-6-1.1-ES.zip
$:~/Downloads rm -rf wordpress-6-1.1-ES.zip
$:~/Downloads mv wordpress /var/www/html
$:~/Downloads cd /var/www/html/wordpress
$:~/Downloads sudo chown -R www-data:www-data .   # Darle todos los archivos de www-data
```

```console
$:/var/www/html/wordpress mv wp-config-sample.php wp-config.php
```
Ese wp-config debe tener los datos que hemos introducido antes en la base de datos mysql.
```php
define('DB_NAME','wordpress');
define('DB_USER','wp-admin');
define('DB_PASSWORD','antonio-recio');
```

```console
$:/var/www/html/wordpress cd /etc/apache2/sites-avaiable
$:/etc/apache2/sites-avaiable cp 000-default.conf wordpress.conf
```
```
<VirtualHost *:80>
	ServerAdmin webmaster@localhost
	DocumentRoot /var/www/html/
</VirtualHost>
```


```console
$:/etc/apache2/sites-avaiable sudo a2ensite wordpress.conf
$:/etc/apache2/sites-avaiable sudo a2dissite 000-default.conf
$:/etc/apache2/sites-avaiable sudo systemctl reload apache2
```





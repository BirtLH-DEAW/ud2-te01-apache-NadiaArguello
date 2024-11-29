# Usa la última versión de Ubuntu como imagen base
FROM ubuntu:latest

# Actualiza la lista de paquetes e instala las actualizaciones disponibles
RUN apt-get update && apt-get upgrade -y

# Instala apache2, openssl y nano
RUN apt-get install -y apache2 openssl nano

# Crea las carpetas nargello, privado y certs
RUN mkdir -p /var/www/html/nargello/privado && mkdir -p /etc/apache2/certs

# Copia los ficheros desde la máquina local al contenedor
COPY html/index.html /var/www/html/
COPY html/css_index /var/www/html/css_index
COPY html/virtualhost.html /var/www/html/nargello
COPY html/css_virtualhost /var/www/html/nargello/css_virtualhost
COPY html/privado.html /var/www/html/nargello/privado
COPY html/css_privado /var/www/html/nargello/privado/css_privado

# Copia y renombra el archivo virtualhost.conf
COPY conf/virtualhost.conf /etc/apache2/sites-available/nargello.conf

# Copia los certificados de seguridad
COPY conf//certs/nargello.key /etc/apache2/certs
COPY conf//certs/nargello.csr /etc/apache2/certs
COPY conf//certs/nargello.crt /etc/apache2/certs

# Modifica el archivo 000-default.conf para añadir ServerName y Redirect
RUN sed -i '/<VirtualHost \*:80>/a \    ServerName localhost\n ' /etc/apache2/sites-available/000-default.conf

# Copia el archivo htpasswd con las credenciales especificadas
RUN htpasswd -cb /etc/apache2/htpasswd deaw deaw

#Habilita el módulo SSL y el sitio nargello.conf, y reinicia Apache
RUN a2enmod ssl && a2ensite nargello.conf

# Exponer puertos 80 y 443
EXPOSE 80 443

# Iniciar Apache
CMD ["apache2ctl", "-D", "FOREGROUND"]

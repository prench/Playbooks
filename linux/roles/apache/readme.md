# apacheall.yml

A la hora de configurar el servicio apache en un servidor nuevo, hay que ejecutar el siguiente playbook apacheall.yml que está localizado en la ruta /etc/ansible/playbooks/linux
Nota: Este playbook no instala el servicio solo lo configura
Descripción
apacheall.yml ejecuta el rol apache en el que están definidas todas las tareas para configurar apache según los estándares de Emasesa. 
De manera resumida, lo que hace es copiar el template del fichero de configuración de apache y configurar un website si la variable está definida.

El rol de apache se encuentra bajo /etc/ansible/playbooks/linux/roles y están definidos los siguientes directorios:
Roles

   |-- tasks
-	main.yml: 
•	comprueba que variables hay que cargar dependiendo del servicio de apache que esté instalado en la maquina (httpd/httpd24-httpd)
•	lanza la tarea distribute-template.yml
•	lanza la tarea configure-apache.yml
•	activa el servicio apache

-	distribute-template.yml
•	copia el fichero de configuración de apache basado en el template que está alojado en el repositorio /etc/ansible/repo/linux/apache 

-	configure-apache.yml
•	configura distintos parámetros del fichero de configuración de apache basado en las variables definidas en ./defaults/main.yml y establece los permisos del fichero
•	habilita/deshabilita módulos de apache basado en las variables definidas en ./defaults/main.yml y establece los permisos del fichero
•	crea el usuario ssii con id 2000 y grupo apache
•	renombra los ficheros de configuración (userdir.conf autoindex.conf welcome.conf) a .NOLOAD
•	distribuye el fichero de configuración del website si la variable apache_template_conf está definida en ./ apache/defaults/main.yml
•	configura diferentes permisos en los directorios definidos en el fichero de configuración del website.
•	Establece Umask 0002 para los servicios de apache y php-fpm


|-- defaults
-	main.yml: 
•	Variables de configuración que aplican tanto a las instalaciones de httpd como httpd24-httpd
•	Localización del template del fichero de configuración de apache (httpd.conf.j2)
•	Identificación de los módulos que se habilitan/deshabilitan
•	Estado del servicio de apache

|-- vars:
-	httpd.yml: 
•	variables asociadas a la instalación del servicio httpd

-	httpd24-httpd.yml
•	variables asociadas a la instalación del servicio httpd24-httpd


|-- handlers:
•	Están definidos los servicios apache y php-fpm por si fuera necesario reiniciarlos tras un cambio realizado por alguna de las tareas del rol



Ejecución
Una vez se ha instalado apache en un servidor nuevo o si se quiere estandarizar la configuración de un apache ya instalado, hay que ejecutar los siguientes pasos:
1.	Confirmar si hay que configurar algún website
Si hay que hacerlo hay que añadir el nombre del website a la variable apache_template_conf que está definida en ./ apache/defaults/main.yml y descomentar la línea.
Eg apache_template_conf: catedraemasesa
2.	Ir a /etc/ansible/playbooks/linux
3.	Ejecutar ansible-playbook apacheall.yml -l <servername>
4.	Si hubiera que configurar otro website distinto, habría que volver a editar la variable apache_template_conf y añadir el nombre del nuevo website 
Eg apache_template_conf: aguasevilla
a.	Importante: Ejecutar ansible-playbook apacheconf.yml -l <servername> (este playbook se explica más abajo)
5.	Si no hubiera que configurar otro website, hay dejar comentada la variable apache_template_conf




# apacheconf.yml

Se puede utilizar este playbook para configurar apache según los estándares de Emasesa.
Este playbook solo cambia los valores definidos en ./ apache/defaults/main.yml en el fichero de configuración de apache y modifica el fichero de configuración de websites si está definido y descomentado en ./ apache/defaults/main.yml

Nota: El resto de websites que estén configurados en el servidor no se ven afectados por la ejecución de este playbook
El playbook se encuentra localizado en la ruta /etc/ansible/playbooks/linux

Descripción
apacheconf.yml utiliza ficheros del rol de apache para configurar apache según los estándares de Emasesa. 
La principal diferencia con apacheall.yml es que no copia el template alojado en el repositorio de apache.

Por defecto, carga las variables definidas en ./apache/defaults/main.yml y dependiendo de la versión del servicio apache instalado, carga las variables asociadas (ya sean ./apache/vars/httpd.yml o ./apache/vars/httpd24-httpd.yml )

Ejecuta las tareas del fichero ./apache/tasks/configure-apache.yml


Ejecución
Para configurar apache en un servidor ya instalado o configurar un nuevo website hay que ejecutar los siguientes pasos:
1.	Confirmar si hay que configurar algún website
Si hay que hacerlo hay que añadir el nombre del website a la variable apache_template_conf que está definida en ./ apache/defaults/main.yml y descomentar la línea.
Eg apache_template_conf: catedraemasesa
2.	Ir a /etc/ansible/playbooks/linux
3.	Ejecutar ansible-playbook apacheconf.yml -l <servername>
Si hubiera que configurar otro website distinto, habría que volver a editar la variable apache_template_conf y añadir el nombre del nuevo website 
Eg apache_template_conf: aguasevilla
4.	Ejecutar ansible-playbook apacheconf.yml -l <servername> 
5.	Si no hubiera que configurar otro website, hay dejar comentada la variable apache_template_conf



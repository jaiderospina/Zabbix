Instalación de Zabbix 6.4 en CentOS 9

```markdown
# Instalación de Zabbix 6.4 en CentOS 9

Este tutorial describe cómo instalar **Zabbix 6.4** en una máquina **CentOS 9** desde cero.

## Pasos de instalación:

1. **Configura SELinux en modo permisivo**:
   - Abre el archivo `/etc/selinux/config` y establece `SELINUX=permissive`.
   - Reinicia la máquina o ejecuta `setenforce 0` para aplicar los cambios.

2. **Instala el repositorio de Zabbix**:
   - Edita el archivo `/etc/yum.repos.d/epel.repo` y agrega la siguiente línea:
     ```
     excludepkgs=zabbix*
     ```
   - Luego, instala el repositorio de Zabbix:
     ```
     # rpm -Uvh ¹
     # dnf clean all
     ```

3. **Instala el servidor, frontend y agente de Zabbix**:
   ```
   # dnf install zabbix-server-mysql zabbix-web-mysql zabbix-agent
   ```

4. **Instala y configura la base de datos (MariaDB)**:
   - Instala MariaDB 10.6:
     ```
     # dnf install mariadb-server
     ```
Reinicia la base de datos y establece una contraseña para el usuario `root`:
     ```
     # systemctl start mariadb
     # mysql_secure_installation
     ```
Crea una base de datos para Zabbix:
     ```
     # mysql -u root -p
     > CREATE DATABASE zabbix CHARACTER SET utf8 COLLATE utf8_bin;
     > GRANT ALL PRIVILEGES ON zabbix.* TO 'zabbix'@'localhost' IDENTIFIED BY 'tu_contraseña';
     > FLUSH PRIVILEGES;
     > EXIT;
     ```
   - Importa el esquema inicial y los datos:
     ```
     # zcat /usr/share/doc/zabbix-server-mysql*/create.sql.gz | mysql -u zabbix -p zabbix
     ```
   - Actualiza la contraseña de la base de datos en el archivo de configuración de Zabbix:
     ```
     # nano /etc/zabbix/zabbix_server.conf
     DBPassword=tu_contraseña
     ```

5. **Inicia los procesos del servidor y el agente de Zabbix**:
   ```
   # systemctl start zabbix-server zabbix-agent httpd
   # systemctl enable zabbix-server zabbix-agent httpd
   ```

Ahora deberías tener **Zabbix 6.4** instalado y funcionando en tu máquina CentOS 9. Puedes acceder al frontend de Zabbix a través de tu navegador web utilizando la dirección IP de la máquina y el puerto 80 o 443.
```

Recuerda reemplazar `tu_contraseña` con la contraseña deseada para la base de datos. ¡Buena suerte con tu instalación de Zabbix! 🚀

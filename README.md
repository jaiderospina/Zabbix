Por supuesto, aquí tienes el contenido en formato **Markdown** para publicar en GitHub:

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
   - Reinicia la base de datos y establece una contraseña para el usuario `root`:
     ```
     # systemctl start mariadb
     # mysql_secure_installation
     ```
   - Crea una base de datos para Zabbix:
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

¡Listo! Ahora deberías tener **Zabbix 6.4** instalado y funcionando en tu máquina CentOS 9. Puedes acceder al frontend de Zabbix a través de tu navegador web utilizando la dirección IP de la máquina y el puerto 80 o 443. Si necesitas más ayuda, no dudes en preguntar.
```

Recuerda reemplazar `tu_contraseña` con la contraseña deseada para la base de datos. ¡Buena suerte con tu instalación de Zabbix! 🚀

Origen: Conversación con Bing, 11/2/2024
(1) How to convert bash shell script to Markdown?. https://unix.stackexchange.com/questions/185657/how-to-convert-bash-shell-script-to-markdown.
(2) 4 Markdown tools for the Linux command line | Opensource.com. https://opensource.com/article/20/3/markdown-apps-linux-command-line.
(3) unix - Create text file and fill it using bash - Stack Overflow. https://stackoverflow.com/questions/4662938/create-text-file-and-fill-it-using-bash.
(4) Creating a command which creates a markdown list with (merged) changes .... https://dev.to/netsi1964/creating-a-command-which-creates-a-markdown-list-with-merged-changes-for-a-git-branch-4ahe.
(5) How to highlight bash/shell commands in markdown?. https://stackoverflow.com/questions/20303826/how-to-highlight-bash-shell-commands-in-markdown.

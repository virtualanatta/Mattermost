# Guía de Instalación de Mattermost en Ubuntu (Self-Hosted)

Esta guía documenta el proceso paso a paso para instalar Mattermost en un servidor Ubuntu, utilizando PostgreSQL como base de datos y Nginx como Proxy Inverso.

##  Requisitos Previos
* Servidor Ubuntu
* Máquina cliente o dispositivo
* Acceso con privilegios `sudo`

---

## 1. Actualización del Sistema
Antes de comenzar, es fundamental asegurar que todos los paquetes estén actualizados.

sudo apt update
sudo apt upgrade -y
## 2. Instalación de la Base de Datos
Mattermost requiere una base de datos para almacenar usuarios y mensajes.

## Instalar PostgreSQL:

sudo apt install postgresql postgresql-contrib -y
## Acceder a la consola de Postgres:
sudo -u postgres psql
## Crear base de datos y usuario:
<img width="272" height="81" alt="image" src="https://github.com/user-attachments/assets/2468f4f4-b703-48fb-89d4-7af85affed3e" />

SQL
CREATE DATABASE mattermost; 
CREATE USER mmuser WITH PASSWORD 'admin'; 
GRANT ALL PRIVILEGES ON DATABASE mattermost to mmuser;

*\q Para salir

# 3. Creación del Usuario de Sistema
Por seguridad, Mattermost no debe ejecutarse como root. Creamos un usuario dedicado.
sudo useradd -r -m -U -d /opt/mattermost mattermost
Explicación de los flags:
-r: Crea un usuario de sistema
-m: Crea Home en /opt/mattermost
-U: Crea un grupo con el mismo nombre
-d: Define la ruta específica del directorio

# 4. Descarga e Instalación de Mattermost
Descargamos el binario oficial y preparamos las carpetas de datos.

## Descargar el paquete:
wget [https://releases.mattermost.com/9.5.1/mattermost-9.5.1-linux-amd64.tar.gz](https://releases.mattermost.com/9.5.1/mattermost-9.5.1-linux-amd64.tar.gz)
## Extraer el contenido:

tar -xvzf mattermost-9.5.1-linux-amd64.tar.gz
## Mover a la ubicación final:

sudo mv mattermost /opt
## Crear directorio de almacenamiento de archivos:

sudo mkdir /opt/mattermost/data
# 5. Configuración de Permisos
## Damos la propiedad de la carpeta al usuario que creamos anteriormente.

sudo chown -R mattermost:mattermost /opt/mattermost
# 6. Configuración de la Base de Datos (config.json)
Debemos editar el archivo de configuración para que Mattermost sepa cómo conectar con Postgres.
sudo nano /opt/mattermost/config/config.json


## Cambios a realizar:

## Cambiar "DriverName": "mysql" por "DriverName": "postgres".

En "DataSource", poner la cadena de conexión:
"postgres://mmuser:tu_contraseña@localhost:5432/mattermost?sslmode=disable&connect_timeout=10"
<img width="1052" height="78" alt="Captura de pantalla 2026-03-02 162722" src="https://github.com/user-attachments/assets/9dcc8b75-316e-4e83-bd67-23b27b71a0e9" />
7. Creación del Servicio en Systemd
Para que Mattermost se inicie automáticamente con el sistema, creamos un archivo de servicio.

sudo nano /lib/systemd/system/mattermost.service
Pegar el siguiente contenido:
<img width="466" height="276" alt="image" src="https://github.com/user-attachments/assets/f2502d72-5d57-4b15-8054-bc0d60452c6f" />

## Activar y arrancar el servicio:
sudo systemctl daemon-reload
sudo systemctl enable mattermost
sudo systemctl start mattermost
# 8. Configuración de Nginx (Proxy Inverso)
Nginx recibirá las peticiones web y las enviará a Mattermost.

## Instalar Nginx:

sudo apt install nginx -y
## Crear archivo de configuración dcoca:

sudo nano /etc/nginx/sites-available/dcoca

## Configuración recomendada:
Nginx
<img width="347" height="127" alt="image" src="https://github.com/user-attachments/assets/a070a1e1-5a96-463e-9de2-501d1ce94495" />


## Activar configuración y limpiar el default:
sudo ln -s /etc/nginx/sites-available/dcoca /etc/nginx/sites-enabled/dcoca
sudo rm /etc/nginx/sites-enabled/default
sudo nginx -t
sudo systemctl restart nginx

# 9. Verificación Final
Para comprobar que todo funciona, el puerto 8065 debe estar en escucha y Nginx operativo.

sudo ss -tlpn | grep 8065
<img width="552" height="32" alt="image" src="https://github.com/user-attachments/assets/1018bd01-590b-450d-86a2-87b9a859b6cf" />

sudo systemctl status mattermost
<img width="538" height="172" alt="image" src="https://github.com/user-attachments/assets/4c187da7-163b-4248-a1f3-cab2045077a5" />

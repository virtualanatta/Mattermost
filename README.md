# Mattermost

Guía de instalación y configuración de Mattermost.

## 📋 Requisitos Previos

- Docker y Docker Compose instalados
- Al menos 4 GB de RAM disponible
- Puertos 80 y 443 disponibles
- Linux, macOS o Windows con WSL2

## 🚀 Instalación Rápida

### 1. Clonar el Repositorio
```bash
git clone https://github.com/virtualanatta/Mattermost.git
cd Mattermost
```

### 2. Configurar Variables de Entorno
```bash
cp .env.example .env
# Editar .env con tus valores
```

### 3. Iniciar Servicios
```bash
docker-compose up -d
```

### 4. Acceder a Mattermost
- URL: `http://localhost:8065`
- Usuario inicial: admin
- Contraseña: mossmoss

## ⚙️ Configuración Principal

### Base de Datos
- PostgreSQL configurada automáticamente
- Datos persistentes en volumen `mattermost-data`

### Puertos
- **8065**: Aplicación web
- **3306**: PostgreSQL (interno)

## 📁 Estructura del Proyecto

```
Mattermost/
├── docker-compose.yml    # Configuración de servicios
├── .env.example          # Variables de ejemplo
├── config/               # Archivos de configuración
└── README.md             # Este archivo
```

## 🔧 Comandos Útiles

```bash
# Parar los servicios
docker-compose down

# Ver logs
docker-compose logs -f

# Reiniciar
docker-compose restart

# Limpiar todo (datos incluidos)
docker-compose down -v
```

## 📚 Documentación Oficial

- [Mattermost Docs](https://docs.mattermost.com)
- [Docker Setup](https://docs.mattermost.com/deployment/docker)

## 📝 Licencia

Este proyecto sigue la licencia de Mattermost.
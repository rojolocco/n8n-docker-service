# Servicio n8n en Producción

Este directorio contiene la configuración para desplegar **n8n** en un entorno de producción utilizando Docker Compose. **n8n** es una herramienta de automatización de flujos de trabajo que permite conectar diversas aplicaciones y servicios.

---

## **Estructura de Archivos**

```plaintext
n8n/
├── .env.prod                  # Variables de entorno para producción
├── docker-compose.yml         # Docker Compose para despliegue en producción
```

---

## **Requisitos Previos**

1. **Docker y Docker Compose** deben estar instalados.
2. **Red compartida** (`caddy_network`) creada previamente:

   ```bash
   docker network create caddy_network
   ```

3. Asegúrate de que el servicio de **Postgres** esté levantado y accesible en la red `caddy_network`.

---

## **Configuración de Variables de Entorno**

Las variables de entorno para este despliegue se definen en el archivo `.env.prod`. Algunas de las variables clave son:

### **`.env.prod`**

```plaintext
# Variables para PostgreSQL
POSTGRES_USER=prod_user
POSTGRES_PASSWORD=prod_password
POSTGRES_DB=prod_db_n8n

# Configuración de n8n
DB_TYPE=postgresdb
DB_POSTGRESDB_HOST=postgres
DB_POSTGRESDB_PORT=5432
DB_POSTGRESDB_DATABASE=prod_db_n8n
DB_POSTGRESDB_USER=prod_user
DB_POSTGRESDB_PASSWORD=prod_password

# Autenticación básica para n8n
N8N_BASIC_AUTH_ACTIVE=true
N8N_BASIC_AUTH_USER=prod_user
N8N_BASIC_AUTH_PASSWORD=prod_password

# Clave de cifrado para datos sensibles
N8N_ENCRYPTION_KEY=tu_clave_de_cifrado_secreta

# URL pública para webhooks de n8n
N8N_URL=https://mi-dominio.com

# Enforce permissions on settings file
N8N_ENFORCE_SETTINGS_FILE_PERMISSIONS=true
```

> **Nota**: Asegúrate de proteger este archivo y no incluirlo en el control de versiones.

---

## **Uso**

### **Levantar el servicio en producción**

Para levantar **n8n** en producción, utiliza el siguiente comando:

```bash
docker-compose -f docker-compose.yml up -d
```

### **Detener el servicio**

Para detener el servicio:

```bash
docker-compose -f docker-compose.yml down
```

### **Verificar el estado del contenedor**

Para verificar que el contenedor de **n8n** está corriendo:

```bash
docker ps
```

---

## **Acceso a n8n**

Puedes acceder a la interfaz web de **n8n** en `http://<tu_ip>:5678`. Para autenticarte, utiliza las credenciales definidas en el archivo `.env.prod`.

---

## **Persistencia de Datos**

Los datos de **n8n** se almacenan en el volumen `n8n_storage_prod`. Este volumen asegura que los datos no se pierdan al detener o reiniciar el contenedor.

### **Ubicación del Volumen**

El volumen se define en el archivo `docker-compose.yml` y es gestionado por Docker.

Para eliminar los datos almacenados:

```bash
docker volume rm n8n_storage_prod
```

> **Advertencia**: Esto eliminará todos los flujos y configuraciones guardadas.

---

## **Solución de Problemas**

### **Error: `There was an error initializing DB`**

- Verifica que las credenciales y el nombre de la base de datos en `.env.prod` sean correctos.
- Asegúrate de que el servicio de **Postgres** esté en ejecución y accesible en la red `caddy_network`.

### **Advertencia: `Permissions 0644 for n8n settings file are too wide`**

- Asegúrate de que el archivo de configuración tenga permisos seguros:

  ```bash
  docker exec -it n8n_prod chmod 600 /home/node/.n8n/config
  ```

- Alternativamente, puedes configurar `N8N_ENFORCE_SETTINGS_FILE_PERMISSIONS=true` en `.env.prod`.

---

## **Contribución**

Si deseas mejorar esta configuración o agregar nuevas funcionalidades, sigue estos pasos:

1. Haz un fork de este repositorio.
2. Realiza tus cambios en una nueva rama.
3. Envía un pull request con tus mejoras.

---

## **Licencia**

Este proyecto está bajo la licencia MIT. Consulta el archivo `LICENSE` para más detalles.

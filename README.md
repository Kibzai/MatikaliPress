# MatikaliPress
Matikali WordPress Integration

## Entornos y Configuración

### 📌 Entornos

1. **Local (Windows)**  
   - Solo para conectarte al servidor vía SSH y administración básica.  
   - No tendrá WordPress instalado, solo acceso remoto.  

2. **Local (Ubuntu)**  
   - Entorno de pruebas con WordPress.  
   - Puede incluir **2 repositorios** para desarrollo independiente.  

3. **UAT (User Acceptance Testing)**  
   - Entorno donde **diseñadores** realizan cambios y pruebas visuales.  

4. **Cliente**
   - **DEV:** Subida de un WordPress relajado para pruebas internas del cliente.  
   - **PROD:** Subidas cuidadosas con foco en **SEO** y respaldos previos.  

---

## 🔑 1. Conectarte al servidor

1. Conéctate por **SSH**:

   ```bash
   ssh usuario@IP_DEL_SERVIDOR

2. Si usas **Fortinet VPN**, asegúrate de:

   * Activar el túnel antes de conectarte.
   * Confirmar que puedes hacer ping al servidor.

3. Si logras conectarte, revisa el **prompt** y verifica que no haya errores.

---

## 🛠 2. Instalar LAMP (Linux, Apache, MySQL, PHP) en Ubuntu

> Ubuntu viene sin entorno LAMP, lo instalamos paso a paso.

**Paso 1: Actualizar paquetes**

```bash
sudo apt update && sudo apt upgrade -y
```

**Paso 2: Instalar Apache**

```bash
sudo apt install apache2 -y
```

**Paso 3: Instalar MySQL**

```bash
sudo apt install mysql-server -y
sudo mysql_secure_installation
```

Configura contraseña de root y opciones de seguridad (puedo indicarte valores recomendados si lo necesitas).

**Paso 4: Instalar PHP y extensiones necesarias**

```bash
sudo apt install php libapache2-mod-php php-mysql php-curl php-gd php-mbstring php-xml php-xmlrpc php-zip -y
```

---

## 📥 3. Descargar y configurar WordPress

**Paso 1: Limpiar carpeta**

```bash
cd /var/www/html
sudo rm index.html
```

**Paso 2: Descargar WordPress**

```bash
wget https://wordpress.org/latest.tar.gz
tar -xvzf latest.tar.gz
sudo mv wordpress/* .
sudo rm -rf wordpress latest.tar.gz
```

**Paso 3: Permisos correctos**

```bash
sudo chown -R www-data:www-data /var/www/html
sudo chmod -R 755 /var/www/html
```

---

## 🗄 4. Crear base de datos para WordPress

1. Entrar a MySQL:

   ```bash
   sudo mysql -u root -p
   ```

2. Dentro de MySQL:

   ```sql
   CREATE DATABASE nombre_de_base_de_datos DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci;
   CREATE USER 'usuario_wp'@'localhost' IDENTIFIED BY 'tu_contraseña_segura';
   GRANT ALL PRIVILEGES ON nombre_de_base_de_datos.* TO 'usuario_wp'@'localhost';
   FLUSH PRIVILEGES;
   EXIT;
   ```

---

## 🌐 5. Configurar WordPress (desde navegador)

1. Ir a:

   ```
   http://IP_DEL_SERVIDOR
   ```

2. Seguir el instalador:

   * Usar la base de datos, usuario y contraseña creados.
   * Configurar nombre del sitio, usuario administrador y contraseña.

3. 🚨 **IMPORTANTE**:

   * Si se asigna un dominio (ej: `conocer.gob.mx`), configurarlo posteriormente.
   * Instalar HTTPS con **Let's Encrypt**:

     ```bash
     sudo apt install certbot python3-certbot-apache -y
     sudo certbot --apache -d tudominio.com -d www.tudominio.com
     ```

---

## 📂 Flujo de trabajo por entorno

| Entorno       | Objetivo                 | Consideraciones              |
| ------------- | ------------------------ | ---------------------------- |
| Local Windows | SSH y administración     | Sin WordPress, solo terminal |
| Local Ubuntu  | Desarrollo y pruebas     | Puede usar 2 repositorios    |
| UAT           | Pruebas de diseño        | Cambios por diseñadores      |
| Cliente DEV   | Test interno del cliente | Subidas rápidas              |
| Cliente PROD  | Producción real          | Respaldar y cuidar SEO       |

---

## 💾 Respaldo antes de producción

Antes de subir cambios a **PROD**:

1. Realizar **backup** de base de datos:

   ```bash
   mysqldump -u root -p nombre_de_base_de_datos > backup.sql
   ```
2. Copiar carpeta `/var/www/html`:

   ```bash
   sudo tar -czvf backup_html.tar.gz /var/www/html
   ```
3. Subir cambios y probar.

# VentasFix

Sistema de gestión comercial para una empresa de productos de iluminación. Permite administrar usuarios internos, productos y clientes B2B mediante una interfaz web con autenticación JWT.

Construido con **Laravel 11**, **Bootstrap 5** y una API REST propia consumida desde el frontend con JavaScript vanilla.

---

## Tecnologías

| Capa | Tecnología |
|---|---|
| Backend | PHP 8.2+, Laravel 11 |
| Autenticación | tymon/jwt-auth 2.2 (JWT HS256) |
| Frontend | Blade, Bootstrap 5, JavaScript ES6 (fetch) |
| Base de datos | MySQL |
| Assets | Vite 6, Tailwind CSS (configurado), Sneat Admin Theme |
| Íconos | Tabler Icons, Boxicons |

---

## Características

### Autenticación
- Login mediante formulario web o endpoint `POST /api/login`
- Al autenticarse, el sistema entrega un **token JWT** (TTL: 60 minutos)
- El token se ingresa manualmente en cada módulo para autorizar las llamadas a la API

### Dashboard
- Vista con contadores en tiempo real de usuarios, productos y clientes registrados

### Usuarios (`/usuarios/web`)
- CRUD completo vía API REST
- Validación de RUT chileno (formato `XX.XXX.XXX-X`)
- Email generado automáticamente: `nombre.apellido@ventasfix.cl`
- Campos: RUT, nombre, apellido, contraseña

### Productos (`/productos/web`)
- CRUD completo vía API REST
- SKU con formato `LUZxxx` (p. ej. `LUZ001`), inmutable tras creación
- Precio de venta calculado automáticamente: `precio_neto × 1.19` (IVA 19%)
- Gestión de stock con niveles: actual, mínimo, bajo y alto
- Vista detallada con previsualización de imagen

### Clientes (`/clientes/web`)
- CRUD completo vía API REST
- Validación de RUT empresa chileno
- Campos: RUT empresa, razón social, rubro, teléfono, dirección, nombre y email de contacto

---

## Instalación y configuración

### Requisitos previos
- PHP 8.2+
- Composer
- Node.js + npm
- MySQL

### Pasos

```bash
# 1. Instalar dependencias PHP
composer install

# 2. Instalar dependencias Node
npm install

# 3. Configurar el entorno
cp .env.example .env
# Editar .env: DB_DATABASE=ventasfix, DB_USERNAME, DB_PASSWORD

# 4. Generar clave de aplicación
php artisan key:generate

# 5. Generar clave JWT
php artisan jwt:secret

# 6. Ejecutar migraciones
php artisan migrate

# 7. Poblar la base de datos (5 usuarios, 5 clientes, 5 productos)
php artisan db:seed

# 8. Compilar assets
npm run build
```

### Servidor de desarrollo

```bash
composer run dev
```

Inicia en paralelo: `php artisan serve`, Vite HMR, queue worker y log viewer.

---

## Rutas principales

### Web

| URL | Descripción |
|---|---|
| `/` | Página de bienvenida |
| `/login` | Formulario de login (muestra el token JWT al autenticarse) |
| `/dashboard` | Panel con contadores de entidades |
| `/usuarios/web` | Gestión de usuarios |
| `/productos/web` | Gestión de productos |
| `/clientes/web` | Gestión de clientes |

### API REST (requiere `Authorization: Bearer <token>`)

| Método | Endpoint | Descripción |
|---|---|---|
| POST | `/api/login` | Obtener token JWT |
| GET/POST | `/api/usuarios` | Listar / Crear usuarios |
| GET/PUT/DELETE | `/api/usuarios/{id}` | Ver / Editar / Eliminar usuario |
| GET/POST | `/api/productos` | Listar / Crear productos |
| GET/PUT/DELETE | `/api/productos/{id}` | Ver / Editar / Eliminar producto |
| GET/POST | `/api/clientes` | Listar / Crear clientes |
| GET/PUT/DELETE | `/api/clientes/{id}` | Ver / Editar / Eliminar cliente |

---

## Estructura del proyecto

```
app/
├── Http/Controllers/
│   ├── api/                  # Controladores JSON (JWT)
│   │   ├── UsuarioController.php
│   │   ├── ProductoController.php
│   │   └── ClienteController.php
│   ├── LoginController.php
│   ├── DashboardController.php
│   ├── UsuarioWebController.php
│   ├── ProductoWebController.php
│   └── ClientesWebController.php
└── Models/
    ├── Usuario.php
    ├── Producto.php
    └── Cliente.php

database/
├── migrations/
├── seeders/
└── factories/

public/js/
├── usuarios.js
├── productos.js
└── clientes.js

resources/views/
├── login.blade.php
├── dashboard.blade.php
├── layouts/
├── usuarios/
├── productos/
└── clientes/
```

---

## Tests

```bash
php artisan test
```

---

## Licencia

MIT

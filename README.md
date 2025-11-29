# 📚 LibroTech Database

Base de datos PostgreSQL para sistema de gestión de biblioteca digital.

## 🎯 Descripción

LibroTech Database es un sistema de base de datos relacional diseñado para gestionar el inventario de libros, autores, categorías y editoriales de una biblioteca o tienda de libros.

## 🗂️ Estructura de la Base de Datos

### Diagrama de Entidad-Relación

![Imagen de WhatsApp 2025-11-28 a las 01 05 38_a515a085](https://github.com/user-attachments/assets/6c826736-20e9-4e17-97e7-27e538299803)


### Modelo de Datos

El sistema implementa las siguientes relaciones:

- **Autores → Libros**: Un autor puede tener múltiples libros (1:N)
- **Categorías → Libros**: Una categoría puede tener múltiples libros (1:N)
- **Editoriales → Libros**: Una editorial puede publicar múltiples libros (1:N)

### Tablas

#### `autores`
| Campo | Tipo | Restricciones | Descripción |
|-------|------|---------------|-------------|
| id | SERIAL | PRIMARY KEY | Identificador único |
| nombre | VARCHAR(255) | NOT NULL | Nombre completo del autor |
| pais | VARCHAR(100) | - | País de origen |
| fecha_nacimiento | DATE | - | Fecha de nacimiento |
| biografia | TEXT | - | Biografía del autor |

#### `categorias`
| Campo | Tipo | Restricciones | Descripción |
|-------|------|---------------|-------------|
| id | SERIAL | PRIMARY KEY | Identificador único |
| nombre | VARCHAR(100) | NOT NULL | Nombre de la categoría |
| descripcion | TEXT | - | Descripción detallada |

#### `editoriales`
| Campo | Tipo | Restricciones | Descripción |
|-------|------|---------------|-------------|
| id | SERIAL | PRIMARY KEY | Identificador único |
| nombre | VARCHAR(255) | NOT NULL | Nombre de la editorial |
| pais | VARCHAR(100) | - | País de origen |
| fundacion | INTEGER | - | Año de fundación |
| direccion | VARCHAR(255) | - | Dirección física |

#### `libros`
| Campo | Tipo | Restricciones | Descripción |
|-------|------|---------------|-------------|
| id | SERIAL | PRIMARY KEY | Identificador único |
| titulo | VARCHAR(255) | NOT NULL | Título del libro |
| autor_id | INTEGER | FK → autores(id) | Referencia al autor |
| categoria_id | INTEGER | FK → categorias(id) | Referencia a categoría |
| editorial_id | INTEGER | FK → editoriales(id) | Referencia a editorial |
| precio | DECIMAL(10,2) | - | Precio del libro |
| stock | INTEGER | DEFAULT 0 | Cantidad disponible |
| isbn | VARCHAR(20) | - | Código ISBN |
| año | INTEGER | - | Año de publicación |

**Tablas Azure:**

<img width="921" height="264" alt="image" src="https://github.com/user-attachments/assets/4ec67383-7960-4027-b928-28d73b45c2dc" />

<img width="921" height="337" alt="image" src="https://github.com/user-attachments/assets/66698825-a8d4-4d83-a01c-c2c260eaadb4" />

<img width="921" height="422" alt="image" src="https://github.com/user-attachments/assets/9fdc630b-fb5b-4940-bb81-7c7e40769da7" />


**Relaciones:**
- `autor_id` → `autores.id` (ON DELETE SET NULL)
- `categoria_id` → `categorias.id` (ON DELETE SET NULL)
- `editorial_id` → `editoriales.id` (ON DELETE SET NULL)

## 🚀 Instalación Local

### Prerequisitos

- PostgreSQL 12 o superior
- Cliente de PostgreSQL (psql, pgAdmin, DBeaver, etc.)

### Pasos de Instalación

1. **Clonar el repositorio**
```bash
git clone <tu-repositorio>
cd librotech-database
```

2. **Crear la base de datos**
```bash
# Conectar a PostgreSQL
psql -U postgres

# Ejecutar el script
\i schema.sql
```

O en una sola línea:
```bash
psql -U postgres -f schema.sql
```

3. **Verificar la instalación**
```bash
psql -U postgres -d librotech
```

```sql
-- Ver todas las tablas
\dt

-- Ver estructura de una tabla
\d libros

-- Verificar relaciones
\d+ libros
```

## ☁️ Despliegue en Azure Database for PostgreSQL

### Método 1: Azure Portal (Interfaz Gráfica)

#### Paso 1: Crear el servidor PostgreSQL

1. Inicia sesión en [Azure Portal](https://portal.azure.com)
2. Clic en **"Crear un recurso"** → Busca **"Azure Database for PostgreSQL"**
3. Selecciona **"Servidor flexible"**
4. Configuración básica:
   - **Suscripción**: Tu suscripción de Azure
   - **Grupo de recursos**: Crea nuevo o usa existente
   - **Nombre del servidor**: `librotech`
   - **Región**: Selecciona la más cercana (ej: East US, Brazil South)
   - **Versión de PostgreSQL**: 14 o 15
   - **Carga de trabajo**: Development 

5. Autenticación:
   - **Método**: Autenticación de PostgreSQL
   - **Nombre de usuario administrador**: `librotechadmin`
   - **Contraseña**: Crea una contraseña segura

6. Redes:
   - **Conectividad**: Acceso público
   - **Permitir acceso desde**: Servicios de Azure
   - **Agregar IP actual**: Sí

7. Clic en **"Revisar y crear"** → **"Crear"**

#### Paso 2: Conectar al servidor

Espera 5-10 minutos a que se aprovisione el servidor.

```bash
# Obtén la cadena de conexión desde el portal
# Ve a tu recurso → Configuración → Cadenas de conexión

# Formato de conexión
psql "host=librotech-db.postgres.database.azure.com port=5432 dbname=postgres user=librotechadmin password=TuPassword sslmode=require"
```

#### Paso 3: Crear la base de datos y tablas

```sql
-- Crear la base de datos
CREATE DATABASE librotech;

-- Salir y reconectar a la nueva base
\q
```

```bash
# Reconectar a la base librotech
psql "host=librotech-db.postgres.database.azure.com port=5432 dbname=librotech user=librotechadmin password=TuPassword sslmode=require"
```

```sql
-- Copiar y pegar el contenido del schema.sql (sin la parte de CREATE DATABASE)
-- O ejecutar:
\i schema.sql
```


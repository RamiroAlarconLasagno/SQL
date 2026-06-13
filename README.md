# SQL Aprendizaje — Guía de Instalación y Setup

Notebook interactivo para aprender SQL desde cero, orientado a entrevistas de trabajo.
Cubre DDL, DML, JOINs, subconsultas, window functions, CTEs, transacciones y diseño de bases de datos.
Motor: **PostgreSQL 16** via Docker.

---

## Requisitos previos

- Python 3.13.3 instalado
- `uv` instalado ([instrucciones](https://docs.astral.sh/uv/getting-started/installation/))
- Docker Desktop instalado (ver sección siguiente)

---

## 1. Instalar Docker Desktop (Windows)

### 1.1 Descarga

Ir a: https://www.docker.com/products/docker-desktop/

Hacer clic en **"Download for Windows"**.

### 1.2 Instalación

1. Ejecutar el instalador descargado
2. Cuando pregunte, dejar marcado **"Use WSL 2 instead of Hyper-V"** (recomendado)
3. Completar la instalación y reiniciar Windows si lo solicita
4. Abrir Docker Desktop y esperar a que el ícono en la barra de tareas quede en verde (**"Engine running"**)

### 1.3 Verificar instalación

Abrir una terminal (CMD, PowerShell o terminal de VS Code) y ejecutar:

```bash
docker --version
```

Debe mostrar algo como `Docker version 26.x.x`.

---

## 2. Configurar el entorno Python

Abrí una terminal **dentro de la carpeta ** y ejecutá:

```bash
# Inicializar el proyecto en la carpeta actual
uv init .

# Instalar dependencias
uv add jupyter psycopg2-binary ipykernel
```

---

## 3. Levantar PostgreSQL con Docker

En la raíz del proyecto ya existe el archivo `docker-compose.yml`.
Ejecutar:

```bash
docker compose up -d
```

Verificar que el contenedor esté corriendo:

```bash
docker ps
```

Debe aparecer `sql_aprendizaje_db` con status `Up`.

### Comandos útiles de Docker

```bash
# Detener la base de datos (los datos se conservan)
docker compose down

# Levantar nuevamente
docker compose up -d

# Ver logs de PostgreSQL
docker logs sql_aprendizaje_db

# Eliminar todo incluyendo datos (reset completo)
docker compose down -v
```

---

## 4. Iniciar Jupyter Notebook

```bash
uv run jupyter notebook
```

Se abre el browser automáticamente en `http://localhost:8888`.

Abrir el archivo `sql_aprendizaje.ipynb`.

---

## 5. Credenciales de conexión

Definidas en `docker-compose.yml`, ya configuradas en el notebook:

| Parámetro | Valor |
|-----------|-------|
| Host | `localhost` |
| Puerto | `5432` |
| Base de datos | `ecommerce` |
| Usuario | `alumno` |
| Contraseña | `alumno123` |

---

## 6. Estructura del proyecto

```
sql-aprendizaje/
├── README.md                  # Este archivo
├── docker-compose.yml         # Configuración de PostgreSQL
├── sql_aprendizaje.ipynb      # Notebook principal
└── pyproject.toml             # Dependencias del proyecto (generado por uv)
```

---

## 7. Temas cubiertos en el notebook

| Sección | Temas |
|---------|-------|
| 01 — Fundamentos | DDL vs DML vs DCL, tipos de datos, CREATE TABLE |
| 02 — Constraints | PK, FK, UNIQUE, NOT NULL, DEFAULT, CHECK |
| 03 — CRUD | INSERT, SELECT, UPDATE, DELETE |
| 04 — Filtros | WHERE, operadores, BETWEEN, IN, LIKE, NULL |
| 05 — Agregación | GROUP BY, HAVING, COUNT, SUM, AVG, MIN, MAX |
| 06 — JOINs | INNER, LEFT, RIGHT, FULL, SELF JOIN, CROSS JOIN |
| 07 — Subconsultas | Escalares, correlacionadas, IN/EXISTS |
| 08 — Normalización | 1NF, 2NF, 3NF — teoría y ejercicios |
| 09 — Índices | Por qué existen, cuándo usarlos, EXPLAIN ANALYZE |
| 10 — Window Functions | ROW_NUMBER, RANK, DENSE_RANK, LAG, LEAD, PARTITION BY |
| 11 — CTEs | WITH, CTEs recursivas |
| 12 — Transacciones | ACID, COMMIT, ROLLBACK, SAVEPOINT |
| 13 — Vistas | CREATE VIEW, casos de uso |
| 14 — CRUD en Python | psycopg2, parámetros seguros, manejo de errores |

---

## Notas

- El schema simula un sistema de e-commerce (clientes, productos, categorías, pedidos)
  para que los ejercicios tengan sentido de negocio real.
- Cada sección del notebook incluye explicación en Markdown, query comentado y resultado visible.
- Las queries de entrevista más frecuentes están marcadas con `🎯 ENTREVISTA`.

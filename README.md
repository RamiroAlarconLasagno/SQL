# sql-aprendizaje/README.md

# SQL Aprendizaje — Guia de Instalacion y Setup

Notebook interactivo para aprender SQL desde cero.
Cubre DDL, DML, JOINs, subconsultas, window functions, CTEs, transacciones y diseno de bases de datos.
Motor: **PostgreSQL 16** via Docker.

---

## Requisitos previos

- Python 3.13 instalado
- `uv` instalado ([instrucciones](https://docs.astral.sh/uv/getting-started/installation/))
- Docker Desktop instalado (ver seccion siguiente)
- VS Code con extension **Jupyter** (`ms-toolsai.jupyter`)

---

## 1. Instalar Docker Desktop (Windows)

### 1.1 Descarga

Ir a: https://www.docker.com/products/docker-desktop/

Hacer clic en **"Download for Windows"**.

### 1.2 Instalacion

1. Ejecutar el instalador descargado
2. Cuando pregunte, dejar marcado **"Use WSL 2 instead of Hyper-V"** (recomendado)
3. Completar la instalacion y reiniciar Windows si lo solicita
4. Abrir Docker Desktop y esperar a que el icono en la barra de tareas quede en verde (**"Engine running"**)

### 1.3 Verificar instalacion

Abrir una terminal (CMD, PowerShell o terminal de VS Code) y ejecutar:

```bash
docker --version
```

Debe mostrar algo como `Docker version 26.x.x`.

---

## 2. Configurar el entorno Python

Abrir una terminal **dentro de la carpeta del proyecto** y ejecutar:

```bash
uv add psycopg[binary] sqlalchemy ipykernel
```

---

## 3. Levantar PostgreSQL con Docker

En la raiz del proyecto ya existe el archivo `docker-compose.yml`.
Ejecutar:

```bash
docker compose up -d
```

Verificar que el contenedor este corriendo:

```bash
docker ps
```

Debe aparecer `sql_aprendizaje_db` con status `Up`.

### Comandos utiles de Docker

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

## 4. Abrir el notebook en VS Code

1. Abrir VS Code en la carpeta del proyecto
2. Abrir el notebook segun el tema a trabajar (ver seccion 7)
3. Seleccionar el kernel: `Ctrl+Shift+P` → **Jupyter: Select Kernel** → **Python Environments** → seleccionar el `.venv` del proyecto
4. Ejecutar las celdas de la seccion 0 para verificar la conexion

---

## 5. Credenciales de conexion

Definidas en `docker-compose.yml`, ya configuradas en los notebooks:

| Parametro | Valor |
|-----------|-------|
| Host | `localhost` |
| Puerto | `5433` |
| Base de datos | `ecommerce` |
| Usuario | `alumno` |
| Contrasena | `alumno123` |

---

## 6. Estructura del proyecto

```
sql-aprendizaje/
├── README.md                  # Este archivo
├── docker-compose.yml         # Configuracion de PostgreSQL
├── sql_psycopg.ipynb          # SQL puro con psycopg v3
├── sql_sqlalchemy.ipynb       # ORM y Core con SQLAlchemy 2
├── sql_avanzado.ipynb         # Temas avanzados con psycopg y SQLAlchemy
└── pyproject.toml             # Dependencias del proyecto (generado por uv)
```

---

## 7. Temas cubiertos en los notebooks

### `sql_psycopg.ipynb` — SQL puro con psycopg v3

| Seccion | Temas |
|---------|-------|
| 01 — Fundamentos | DDL vs DML vs DCL, tipos de datos, CREATE TABLE |
| 02 — Constraints | PK, FK, UNIQUE, NOT NULL, DEFAULT, CHECK |
| 03 — CRUD | INSERT, SELECT, UPDATE, DELETE |
| 04 — Filtros | WHERE, operadores, BETWEEN, IN, LIKE, NULL |
| 05 — Agregacion | GROUP BY, HAVING, COUNT, SUM, AVG, MIN, MAX |
| 06 — JOINs | INNER, LEFT, RIGHT, FULL, SELF JOIN, CROSS JOIN |
| 07 — Subconsultas | Escalares, correlacionadas, IN/EXISTS |
| 08 — Normalizacion | 1NF, 2NF, 3NF — teoria y ejercicios |

### `sql_sqlalchemy.ipynb` — ORM y Core con SQLAlchemy 2

| Seccion | Temas |
|---------|-------|
| 00 — Conexion | Engine, Session, text(), ejecutar_query() |
| 01 — DDL con Core | MetaData, Table, Column, tipos, constraints, create_all |
| 02 — INSERT con Core | insert(), executemany, returning() |
| 03 — SELECT con Core | select(), where(), join(), group_by(), having(), func |
| 04 — Modelos ORM | DeclarativeBase, Mapped, mapped_column, relationship |
| 05 — CRUD con ORM | session.add(), session.get(), update(), delete(), manejo de errores |
| 06 — Consultas avanzadas | Filtros, joins, subconsultas con ORM |
| 07 — Eager vs Lazy loading | selectinload, joinedload, lazy loading |
| 08 — Transacciones | Session, commit, rollback, savepoint |
| 09 — Migraciones | Alembic: init, revision, upgrade, downgrade |

### `sql_avanzado.ipynb` — Temas avanzados con psycopg y SQLAlchemy

| Seccion | Temas |
|---------|-------|
| 09 — Indices | Por que existen, cuando usarlos, EXPLAIN ANALYZE |
| 10 — Window Functions | ROW_NUMBER, RANK, DENSE_RANK, LAG, LEAD, PARTITION BY |
| 11 — CTEs | WITH, CTEs recursivas |
| 12 — Transacciones | ACID, COMMIT, ROLLBACK, SAVEPOINT |
| 13 — Vistas | CREATE VIEW, casos de uso |

Cada seccion muestra el mismo concepto implementado con ambas librerias
para comparar el enfoque de SQL puro vs la abstraccion de SQLAlchemy.

---

## Notas

- El schema simula un sistema de e-commerce (clientes, productos, categorias, pedidos)
  para que los ejercicios tengan sentido de negocio real.
- Cada seccion incluye explicacion en Markdown, query comentado y resultado visible.
- `sql_psycopg.ipynb` — SQL puro, control total, mas cercano al motor.
- `sql_sqlalchemy.ipynb` — abstraccion ORM, orientado a uso en aplicaciones Python.
- `sql_avanzado.ipynb` — comparacion lado a lado de ambos enfoques en temas avanzados.
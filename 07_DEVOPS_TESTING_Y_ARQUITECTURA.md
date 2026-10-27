# ⚙️ DevOps, Testing & Patrones de Arquitectura de Software

> **Importancia para la Prueba:**  
> Los evaluadores técnicos prestan especial atención a cómo aseguras la **calidad del código**, la **reproducibilidad del entorno** y los **patrones de diseño** elegidos para resolver problemas de escala, mantenibilidad y resiliencia.

---

## 📑 Tabla de Contenidos
1. [DevOps, Contenedores & Entornos](#1-devops-contenedores--entornos)
2. [Estrategias de Testing & Calidad](#2-estrategias-de-testing--calidad)
3. [Herramientas Modernas de Rendimiento (uv & Ruff)](#3-herramientas-modernas-de-rendimiento-uv--ruff)
4. [Los 7 Patrones de Arquitectura Presentes en tus Proyectos](#4-los-7-patrones-de-arquitectura-presentes-en-tus-proyectos)

---

## 1. DevOps, Contenedores & Entornos

### 🐳 Docker & Dockerfile Multi-Stage
* **Concepto:** Empaqueta la aplicación con su runtime, dependencias del sistema operativo y código fuente en una imagen inmutable.
* **Patrón Multi-Stage Build:**
  * Separa la fase de **compilación/construcción** (donde se necesitan compiladores de C, Node, herramientas pesadas) de la fase de **producción** (donde solo se copia el binario o las librerías finales en una imagen ligera como Alpine o Debian Slim).
  * *Beneficio:* Reduce el tamaño de la imagen final de 1.2 GB a menos de 150 MB y elimina vulnerabilidades de seguridad.

### 🐙 Docker Compose
* **Concepto:** Herramienta para definir y ejecutar aplicaciones multicontenedor mediante un archivo `docker-compose.yml`.
* **Mejores Prácticas Implementadas en tus Proyectos:**
  * **Networks:** Redes aisladas (ej. `backend-net`, `data-net`) para que los servicios solo se comuniquen con quienes deben.
  * **Volumes:** Persistencia de datos en disco (`postgres_data:/var/lib/postgresql/data`, `minio_data:/data`) para que la información sobreviva si el contenedor se reinicia.
  * **Healthchecks & Depends_on:** Evitar que la API inicie antes de que la base de datos esté lista para aceptar conexiones:
    ```yaml
    services:
      postgres:
        image: postgres:16-alpine
        healthcheck:
          test: ["CMD-SHELL", "pg_isready -U postgres"]
          interval: 5s
          timeout: 5s
          retries: 5
      api:
        build: .
        depends_on:
          postgres:
            condition: service_healthy
    ```

---

## 2. Estrategias de Testing & Calidad

### 🧪 Pytest, Pytest-Asyncio & Pytest-Cov
* **Pytest:** Framework de pruebas en Python con aserciones simples (`assert x == y`), fixtures reutilizables y parametrización de casos de prueba.
* **Pytest-Asyncio:** Decorador `@pytest.mark.asyncio` para probar endpoints asíncronos y corrutinas de FastAPI:
  ```python
  import pytest
  from httpx import AsyncClient
  from app.main import app

  @pytest.mark.asyncio
  async def test_health_check():
      async with AsyncClient(app=app, base_url="http://test") as ac:
          response = await ac.get("/health")
      assert response.status_code == 200
      assert response.json() == {"status": "healthy"}
  ```
* **Pytest-Cov:** Medición de cobertura de código (*Code Coverage*). En tus proyectos apuntas a más del 80% de cobertura en la lógica de negocio (`pytest --cov=app --cov-report=html`).
* **Pirámide de Testing:**
  1. **Tests Unitarios (Base):** Prueban funciones aisladas sin dependencias de red ni base de datos (usan mocks para APIs y LLMs).
  2. **Tests de Integración (Centro):** Prueban la interacción entre FastAPI, SQLAlchemy y la base de datos real (ej. PostgreSQL en contenedor de test).
  3. **Tests End-to-End / E2E (Cúspide):** Flujos completos desde la petición del usuario hasta la respuesta final procesada.

---

## 3. Herramientas Modernas de Rendimiento

### 🦀 uv (Astral)
* **¿Qué es?:** Gestor de paquetes y dependencias de Python ultrarrápido escrito en Rust, diseñado para reemplazar `pip`, `pip-tools` y `virtualenv`.
* **Ventajas Técnicas:**
  * **Velocidad:** Resuelve dependencias entre 10 y 100 veces más rápido que pip tradicional.
  * **Caché Global Inteligente:** No vuelve a descargar librerías si ya están en el sistema.
  * **Archivo de Bloqueo Inmutable (`uv.lock`):** Garantiza que todo el equipo instale exactamente las mismas sub-dependencias.

### ⚡ Ruff
* **¿Qué es?:** Linter y formateador de código Python extremadamente rápido escrito en Rust (reemplaza a Flake8, Black, isort y Pylint en una sola herramienta).

---

## 4. Los 7 Patrones de Arquitectura Presentes en tus Proyectos

### 🥇 1. Arquitectura Medallón (Medallion Lakehouse)
* **Ubicación:** `pruebas/Pipeline-analitica` y `Antigravit/final_project`.
* **Capas:**
  * **Bronze (Raw / Crudo):** Ingesta fiel de datos desde APIs, archivos CSV o JSON sin transformar. Almacenado en MinIO/S3.
  * **Silver (Cleaned / Limpio):** Datos validados, eliminados nulos y duplicados, tipados y transformados a **Apache Parquet**.
  * **Gold (Business / Agregado):** Tablas de métricas listas para el negocio, KPIs de ventas y modelos analíticos de consulta en DuckDB o Streamlit.
* **Justificación:** Separa la ingesta de la transformación y el consumo, garantizando trazabilidad y linaje de datos.

---

### ⚡ 2. Arquitectura de Microservicios Orientada a Eventos (Event-Driven)
* **Ubicación:** `Antigravit/gimnasio RIWI` y `Antigravit/RIWI OCR`.
* **Flujo:**
  * Microservicio A (Ingesta) recibe el archivo ➔ Sube a S3/MinIO ➔ Publica un mensaje con el ID en AWS SQS.
  * Microservicio B (OCR / Procesamiento) escucha la cola SQS ➔ Descarga de S3 ➔ Ejecuta el OCR ➔ Guarda el resultado en PostgreSQL ➔ Notifica a SQS de finalización.
* **Justificación:** **Desacoplamiento total y escalabilidad independiente**. Si el OCR tarda 5 segundos por imagen, la API de subida no se bloquea ni ralentiza la experiencia del usuario.

---

### 🏛️ 3. Clean Architecture / Arquitectura Hexagonal
* **Ubicación:** `pruebas/Pipeline-analitica/app/backend` y `Antigravit/AgenteIA`.
* **Capas Concéntricas:**
  * **Domain / Entities:** Modelos puros y reglas de negocio sin dependencias externas.
  * **Application / Services:** Casos de uso de la aplicación (ej. `RegisterCustomerUseCase`).
  * **Infrastructure / Adapters:** Controladores FastAPI, repositorios SQLAlchemy, clientes de LLM o MinIO.
* **Regla de Dependencia:** Las capas internas nunca conocen a las capas externas. La lógica de negocio no depende de la base de datos ni del framework web.

---

### 🤖 4. Arquitectura Multi-Agente RAG con Triaje
* **Ubicación:** `Antigravit/Agentes` y `sebdevultra/Agentes`.
* **Roles Especializados:**
  1. **Agente de Triaje / Router:** Analiza la consulta y decide a qué agente especialista delegarla.
  2. **Agente Especialista (Extracción/RAG):** Consulta la base de datos vectorial (Supabase / ChromaDB) y recupera contexto relevante.
  3. **Agente Validador / Guardrail:** Audita la respuesta del especialista para evitar alucinaciones y verificar que cumple con el esquema antes de responder al usuario.

---

### 📶 5. Tiered Fallback Engine (Motor de Inferencia Multi-Nivel)
* **Ubicación:** `Ruta_avanzada_IA/Desarrollo/Sistemas` (*JDS Sentinel AI*).
* **Jerarquía de Ejecución:**
  * **Nivel 1 (Léxico / Simbólico):** Detección directa por reglas (< 1ms, $0.00 costo).
  * **Nivel 2 (Neural Local):** Inferencia con embeddings locales y similitud de coseno en memoria (15-40ms, $0.00 costo).
  * **Nivel 3 (Cloud LLM):** Gemini / OpenAI activado únicamente cuando los niveles previos no superan el umbral de confianza semántica.
* **Beneficio:** Reduce los costos de API en más de un 80% y garantiza disponibilidad incluso sin internet.

---

### 🎮 6. Patrón ECS (Entity-Component-System) & Game Loop
* **Ubicación:** `Ruta_avanzada_IA/Desarrollo/Sistemas/Personales` (*Juego 2D*).
* **Principios:**
  * **Entity:** Identificador simple (un ID de objeto).
  * **Component:** Datos puros sin lógica (ej. `Position`, `Velocity`, `Hitbox`).
  * **System:** Lógica pura que itera sobre todas las entidades que tienen ciertos componentes (ej. `MovementSystem`, `CollisionSystem`).

---

### 🌐 7. Single Page Application (SPA) MVC en Vanilla JS
* **Ubicación:** `Riwi/Proyecto_spa_previa1`, `Riwi/Pruebas SPA simulacro`.
* **Componentes:**
  * **Router (`router.js`):** Escucha cambios de URL (History API o Hash Router) y carga la vista correspondiente sin recargar la página.
  * **Controller (`home.controller.js`):** Gestiona los eventos de usuario, llama al servicio HTTP y actualiza el DOM.
  * **Service (`reservation.service.js`):** Cliente de red que encapsula las peticiones `fetch()` a la API.
  * **View (`homeView.js`):** Retorna el template HTML y renderiza los componentes.

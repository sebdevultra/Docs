# 🧠 Glosario Maestro: Cuándo Usar y Cuándo NO Usar Cada Tecnología

> **Objetivo:** Consulta rápida para tu prueba técnica. Busca aquí cualquier tecnología con `Ctrl + F` para responder con autoridad técnica y justificar tus decisiones de diseño.

---

## 📑 Índice de Categorías
1. [Backend & Web Frameworks](#1-backend--web-frameworks)
2. [Frontend & UI Frameworks](#2-frontend--ui-frameworks)
3. [Data Engineering, Big Data & Orquestación](#3-data-engineering-big-data--orquestación)
4. [Inteligencia Artificial, LLMs, Agentes & RAG](#4-inteligencia-artificial-llms-agentes--rag)
5. [Bases de Datos, Caché & Almacenamiento](#5-bases-de-datos-caché--almacenamiento)
6. [Mensajería, Colas & Eventos Asíncronos](#6-mensajería-colas--eventos-asíncronos)
7. [Librerías Python Clave](#7-librerías-python-clave)
8. [Librerías JavaScript & TypeScript Clave](#8-librerías-javascript--typescript-clave)
9. [DevOps, Testing & Herramientas](#9-devops-testing--herramientas)

---

## 1. Backend & Web Frameworks

### 🚀 FastAPI (Python)
* **¿Qué es?:** Framework ASGI moderno en Python para construir APIs RESTful y WebSockets de alto rendimiento, basado en tipado de Python y Pydantic.
* **✅ Cuándo USARLO:**
  * Microservicios de Inteligencia Artificial, Machine Learning o pipelines de datos en Python.
  * APIs asíncronas con alta concurrencia de I/O (llamadas a base de datos, APIs externas, LLMs en streaming).
  * Cuando necesitas documentación OpenAPI/Swagger generada automáticamente y tipado estricto en requests/responses.
* **❌ Cuándo NO USARLO:**
  * Aplicaciones monolíticas tradicionales con renderizado del lado del servidor (SSR) de templates HTML complejos (mejor usar **Django**).
  * Scripts simples de un solo archivo que no requieren endpoints ni servidor web (usa scripts estándar de Python o CLI con `argparse`).
* **💡 Alternativa:** **Django** (si requieres Admin panel, ORM integrado y auth nativa) o **Express.js** (si todo el ecosistema es JavaScript/Node).
* **💻 Snippet Clave:**
  ```python
  from fastapi import FastAPI
  app = FastAPI(title="AI Service")
  @app.get("/predict")
  async def predict(q: str):
      return {"result": f"processed {q}"}
  ```

---

### 🟢 Express.js (Node.js)
* **¿Qué es?:** Framework minimalista y flexible para crear servidores web y APIs REST en Node.js mediante middlewares.
* **✅ Cuándo USARLO:**
  * Microservicios en JavaScript/TypeScript que coordinan eventos, webhooks, auth liviana o integraciones AWS (SQS, S3).
  * Backends donde el equipo ya domina JavaScript y se busca compartir tipos con el frontend TypeScript.
  * APIs con lógica de negocio I/O intensiva sin cálculos matemáticos pesados en CPU.
* **❌ Cuándo NO USARLO:**
  * Tareas intensivas en CPU (procesamiento de imágenes pesado, cálculos numéricos, inferencia de ML en local) porque bloquean el Event Loop de Node.js.
  * Cuando requieres tipado estricto out-of-the-box sin configurar TypeScript manualmente (mejor usar **NestJS** o **FastAPI**).
* **💡 Alternativa:** **FastAPI** (para ML/Data), **NestJS** (para arquitectura empresarial estructurada en TypeScript).

---

### ⚡ Starlette (Python)
* **¿Qué es?:** El toolkit ASGI ligero sobre el cual está construido FastAPI.
* **✅ Cuándo USARLO:**
  * Cuando necesitas el máximo rendimiento ASGI posible en Python sin la sobrecarga de validación de esquemas de Pydantic.
  * Servicios proxy ultrarrápidos, gateways de WebSockets puros o middlewares personalizados.
* **❌ Cuándo NO USARLO:**
  * La mayoría de APIs REST estándar (usa **FastAPI**, que incluye Starlette + Pydantic + Swagger).

---

## 2. Frontend & UI Frameworks

### ⚛️ React (v18+)
* **¿Qué es?:** Librería/framework declarativo basado en componentes para construir interfaces de usuario interactivas con Virtual DOM.
* **✅ Cuándo USARLO:**
  * Single Page Applications (SPAs) y dashboards dinámicos con estados complejos, interacción continua y múltiples vistas.
  * Cuando necesitas un ecosistema maduro con miles de librerías de componentes (Lucide, Framer Motion, Radix UI).
* **❌ Cuándo NO USARLO:**
  * Sitios web puramente informativos o estáticos donde el SEO inicial es crítico y React puro agrega peso innecesario de JS (usa **Astro** o HTML/CSS puro).
  * Formularios o vistas simples de un examen donde Vanilla JS resuelve la interacción en menos de 50 líneas sin bundler.
* **💡 Alternativa:** **Vanilla JS Modular** (para proyectos livianos/pruebas rápidas), **Next.js** (para SSR y SEO corporativo).

---

### 🎨 Tailwind CSS (v3 / v4)
* **¿Qué es?:** Framework CSS utility-first para diseñar componentes rápidamente combinando clases directamente en el HTML/JSX.
* **✅ Cuándo USARLO:**
  * Prototipado y desarrollo ultrarrápido sin escribir archivos `.css` separados.
  * Creación de diseños responsivos y soporte instantáneo de Dark Mode (`dark:bg-slate-900`).
  * Evitar colisiones de nombres de clases y stylesheets gigantescos no utilizados.
* **❌ Cuándo NO USARLO:**
  * Proyectos donde el equipo prefiere CSS semántico desacoplado o diseño basado estrictamente en CSS Modules.
  * Cuando no se tiene un bundler (Vite/PostCSS) configurado y se requiere código sin dependencias (aunque existe Tailwind CDN para pruebas rápidas).
* **💡 Alternativa:** **CSS3 nativo con CSS Variables** o **Bootstrap**.

---

### 🌐 Three.js (WebGL 3D)
* **¿Qué es?:** Librería JavaScript para crear y renderizar gráficos 3D acelerados por GPU y shaders interactivos sobre HTML5 Canvas.
* **✅ Cuándo USARLO:**
  * Fondos interactivos futuristas, animaciones WebGL, shaders de partículas y experiencias visuales inmersivas (*JDS Sentinel AI*).
  * Visualización de modelos 3D, física espacial o gemelos digitales en la web.
* **❌ Cuándo NO USARLO:**
  * Aplicaciones web estándar de gestión o formularios CRUD (agrega alto consumo de GPU y batería).
  * Gráficos estadísticos simples de barras o líneas (usa **Chart.js** o **Plotly**).
* **💡 Alternativa:** **Chart.js** / **Plotly** (para datos 2D) o **Canvas API 2D nativo** (para juegos sencillos).

---

## 3. Data Engineering, Big Data & Orquestación

### 🌪️ Apache Airflow
* **¿Qué es?:** Plataforma programática de orquestación de flujos de trabajo basada en Grafos Acíclicos Dirigidos (DAGs) en Python.
* **✅ Cuándo USARLO:**
  * Orquestar pipelines ETL/ELT complejos con dependencias entre tareas, reintentos automáticos, SLAs y monitoreo centralizado.
  * Coordinar servicios heterogéneos (ej. extraer de API -> cargar a MinIO -> ejecutar Spark Job -> transformar en DuckDB -> notificar por correo).
* **❌ Cuándo NO USARLO:**
  * **Procesar los datos dentro del propio Airflow**: Airflow es el *director de orquesta*, NO el músico. El procesamiento pesado debe delegarse a Spark, DuckDB, Trino o SQL.
  * Tareas de tiempo real / streaming continuo por debajo del segundo (Airflow está diseñado para ejecución por lotes o batch programado).
* **💡 Alternativa:** **Prefect** o **Dagster** (para pipelines Python más modernos), **Cron / APScheduler** (para un script individual sin dependencias).
* **💻 Snippet Clave:**
  ```python
  from airflow import DAG
  from airflow.operators.python import PythonOperator
  from datetime import datetime

  with DAG("medallion_etl", start_date=datetime(2026, 1, 1), schedule_interval="@daily") as dag:
      task_ingest = PythonOperator(task_id="ingest_bronze", python_callable=my_func)
  ```

---

### 🔥 Apache Spark / PySpark
* **¿Qué es?:** Motor unificado de procesamiento analítico distribuido en memoria para Big Data a gran escala (Terabytes/Petabytes).
* **✅ Cuándo USARLO:**
  * Procesar volúmenes masivos de datos que superan la memoria RAM de una sola máquina física.
  * Transformaciones complejas en la arquitectura Medallón (Bronze a Silver, Silver a Gold) distribuidas en clusters.
  * Ecosistemas con Data Lakes (Parquet, Delta Lake, Iceberg) y queries distribuidas.
* **❌ Cuándo NO USARLO:**
  * Datasets de menos de unos pocos Gigabytes que caben cómodamente en memoria (aquí Spark es mucho más lento por el overhead de la JVM y red; usa **DuckDB** o **Pandas**).
  * Transacciones ACID de baja latencia por fila (usa PostgreSQL).
* **💡 Alternativa:** **DuckDB** (10x más rápido en una sola máquina para datasets de < 100 GB) o **Polars/Pandas**.

---

### 🦆 DuckDB
* **¿Qué es?:** Motor SQL analítico (OLAP) columnar embebido en proceso (el "SQLite de la analítica").
* **✅ Cuándo USARLO:**
  * Consultas SQL ultrarrápidas directamente sobre archivos **Parquet**, **CSV** o **JSON** locales o en S3/MinIO sin levantar ningún servidor.
  * Motores de consulta para dashboards (ej. Streamlit + DuckDB) con agregaciones sobre millones de filas en milisegundos.
  * Pipelines de transformación en una sola máquina con uso óptimo de todos los cores de CPU mediante vectorización.
* **❌ Cuándo NO USARLO:**
  * Cargas OLTP concurrentes con miles de usuarios haciendo escrituras simultáneas (`INSERT/UPDATE/DELETE` fila por fila).
  * Big Data masivo que requiere distribución en múltiples nodos físicos (usa Spark o ClickHouse).
* **💡 Alternativa:** **PostgreSQL** (para OLTP transaccional) o **ClickHouse** (para OLAP distribuido de alto volumen).
* **💻 Snippet Clave:**
  ```python
  import duckdb
  # Consulta directa sobre parquet en disco sin cargarlo todo a memoria
  df = duckdb.query("SELECT category, SUM(total) FROM 's3://gold/sales/*.parquet' GROUP BY category").to_df()
  ```

---

### 📊 Streamlit
* **¿Qué es?:** Framework open-source en Python para convertir scripts de datos en aplicaciones web interactivas en minutos.
* **✅ Cuándo USARLO:**
  * Dashboards de datos analíticos, demos de modelos de Machine Learning y herramientas internas sin escribir HTML, CSS o JS.
  * Visualización rápida de métricas de calidad de datos, linaje y KPIs comerciales (*Pipeline-analitica*).
* **❌ Cuándo NO USARLO:**
  * Aplicaciones comerciales para millones de usuarios externos que requieren autenticación compleja, diseño milimétrico y state management granular.
  * Interfaces SPA que necesitan micro-interacciones personalizadas (usa React).
* **💡 Alternativa:** **FastAPI + React** (para producción profesional) o **Dash / Gradio**.

---

## 4. Inteligencia Artificial, LLMs, Agentes & RAG

### 🧠 Google GenAI SDK (`google-genai` / `@google/genai`)
* **¿Qué es?:** SDK oficial de nueva generación de Google para interactuar con la familia de modelos **Gemini** (Gemini 2.5 Flash, 3.5 Flash, 3.5 Flash-Lite).
* **✅ Cuándo USARLO:**
  * Aplicaciones que requieren la menor latencia posible y bajo costo por token (Gemini Flash-Lite).
  * Grandes ventanas de contexto (hasta 1 millón de tokens) para procesar documentos PDF completos, libros o bases de código enteras.
  * Streaming en tiempo real de tokens para interfaces interactivas de chat y voz.
* **❌ Cuándo NO USARLO:**
  * Tareas que requieren modelos open-source corriendo 100% offline en hardware local sin salida a internet (usa **Ollama** o **PyTorch**).
* **💡 Alternativa:** **OpenAI SDK** o **Anthropic SDK**.
* **💻 Snippet Clave (Python):**
  ```python
  from google import genai
  client = genai.Client()
  response = client.models.generate_content(
      model='gemini-2.5-flash',
      contents='Explica la diferencia entre OLTP y OLAP.'
  )
  print(response.text)
  ```

---

### 🦜🔗 LangChain & LangGraph
* **¿Qué es?:** Frameworks para orquestar flujos de modelos de lenguaje; LangGraph agrega grafos de estado cíclicos y agentes multi-paso.
* **✅ Cuándo USARLO:**
  * Flujos de agentes complejos con bifurcaciones condicionales, memoria conversacional, ciclos de reintento y ejecución de herramientas (*tools*).
  * RAG con división avanzada de documentos, reranking y combinación de múltiples fuentes.
* **❌ Cuándo NO USARLO:**
  * Llamadas simples de prompt-response a un LLM (agrega una capa de abstracción pesada e innecesaria; usa el SDK directo de Google u OpenAI).
  * Cuando el pipeline es lineal y estático (usa scripts simples con llamadas HTTP o funciones nativas).
* **💡 Alternativa:** **Llamaindex** (para RAG intensivo en documentos) o **SDK nativo + Pydantic** (arquitectura ligera).

---

### 🎯 ChromaDB vs Qdrant vs Supabase Vector (pgvector)
* **¿Qué son?:** Motores de bases de datos vectoriales para almacenar embeddings y realizar búsquedas de similitud semántica (k-NN / cosine similarity).
* **✅ Cuándo USAR ChromaDB:** Proyectos locales, prototipos, pruebas técnicas y tests unitarios. Corre embebido en Python sin requerir Docker ni servidores externos.
* **✅ Cuándo USAR Qdrant:** Producción con millones de vectores, filtrado avanzado por metadatos (payload filtering) y requerimientos de latencia de milisegundos en Rust.
* **✅ Cuándo USAR pgvector / Supabase:** Cuando ya usas PostgreSQL para tu data relacional y quieres unificar datos transaccionales y vectores en la misma base de datos sin agregar otro componente de infraestructura.
* **❌ Cuándo NO usar Vector DB:** Búsquedas por coincidencia exacta de palabras clave, IDs o campos categóricos (usa un índice B-Tree tradicional o Full-Text Search de PostgreSQL).

---

### 🔢 Tiktoken
* **¿Qué es?:** Tokenizador ultrarrápido BPE (Byte-Pair Encoding) de OpenAI para el encoding `cl100k_base`.
* **✅ Cuándo USARLO:**
  * Contar tokens con precisión milimétrica antes de enviar una solicitud a la API de un LLM.
  * Estimar y auditar costos económicos en USD en tiempo de ejecución para evitar sorpresas en facturación.
  * Truncar prompts para no exceder límites de contexto o balancear el tamaño de chunks en RAG.
* **❌ Cuándo NO USARLO:**
  * Modelos que no usan la tokenización de OpenAI (como modelos basados en SentencePiece de LLaMA o Gemini, aunque `cl100k_base` sirve como muy buena aproximación).

---

### 🤖 PyTorch (`torch`, `torch.nn`)
* **¿Qué es?:** Framework de Deep Learning líder para tensores y redes neuronales con soporte de GPU/CPU.
* **✅ Cuándo USARLO:**
  * Inferencia local de modelos de embeddings o capas de autoatención personalizadas (*Tiered Fallback Engine*).
  * Entrenamiento o fine-tuning de modelos propios sin depender de APIs de terceros.
* **❌ Cuándo NO USARLO:**
  * Si solo necesitas llamar a un modelo comercial para generar texto (usa la API de Gemini u OpenAI).
  * Si tu problema se resuelve con Machine Learning tabular clásico (usa **Scikit-learn** o **XGBoost**).

---

## 5. Bases de Datos, Caché & Almacenamiento

### 🐘 PostgreSQL
* **¿Qué es?:** Sistema de base de datos relacional orientada a objetos más avanzado, open-source y con cumplimiento estricto de ACID.
* **✅ Cuándo USARLO:**
  * Núcleo transaccional de cualquier aplicación de producción (usuarios, pedidos, autenticación, lógica relacional compleja).
  * Soporte versátil: almacena JSONB, arrays, búsqueda Full-Text y vectores (pgvector).
* **❌ Cuándo NO USARLO:**
  * Almacenamiento analítico de petabytes con consultas de agregación en tiempo real (usa ClickHouse o DuckDB).
  * Datos efímeros de lectura/escritura en microsegundos (usa Redis).

---

### 🪶 SQLite
* **¿Qué es?:** Base de datos relacional serverless, autocontenida y embebida en un solo archivo en disco.
* **✅ Cuándo USARLO:**
  * Aplicaciones de escritorio, tests unitarios rápidos, prototipos y herramientas locales (*churn_monitor.db*).
  * Sistemas embebidos o microservicios que solo requieren un único hilo de escritura.
* **❌ Cuándo NO USARLO:**
  * Cargas de trabajo concurrentes con múltiples clientes escribiendo simultáneamente en red (se bloquea con `database is locked`).

---

### ⚡ Redis
* **¿Qué es?:** Base de datos en memoria (In-Memory Key-Value) de latencia ultra baja (sub-milisegundo).
* **✅ Cuándo USARLO:**
  * Caché de resultados frecuentes, gestión de sesiones de usuario y contadores de Rate Limiting.
  * Broker de mensajería para Celery o Airflow en tareas asíncronas.
* **❌ Cuándo NO USARLO:**
  * Almacenamiento persistente primario de datos relacionales críticos que no pueden tolerar pérdida de memoria ante caídas.

---

### 📦 MinIO (S3 Compatible)
* **¿Qué es?:** Servidor de almacenamiento de objetos distribuido de alto rendimiento, 100% compatible con la API de Amazon S3.
* **✅ Cuándo USARLO:**
  * Entornos locales o privados para crear Data Lakes (capas Bronze, Silver y Gold en formato Parquet/JSON).
  * Almacenar archivos no estructurados (imágenes, PDFs, backups de bases de datos) utilizando el SDK estándar de `boto3`.
* **❌ Cuándo NO USARLO:**
  * Base de datos para consultas transaccionales directas (no reemplaza a PostgreSQL).

---

## 6. Mensajería, Colas & Eventos Asíncronos

### 📬 Apache Kafka vs Redpanda vs AWS SQS
| Herramienta | Tipo | Cuándo USARLO | Cuándo NO USARLO |
| :--- | :--- | :--- | :--- |
| **Apache Kafka** | Distributed Event Streaming | Millones de eventos por segundo, retención histórica, repetición de eventos (*replay*) y múltiples consumidores independientes. | Aplicaciones pequeñas o arquitecturas simples donde la complejidad operativa de Zookeeper/KRaft es excesiva. |
| **Redpanda** | Kafka-compatible en C++ | Mismo caso que Kafka, pero sin JVM ni Zookeeper, con despliegue en un solo binario ultrarrápido y menor consumo de RAM. | Si la empresa exige estrictamente la suite enterprise de Confluent con conectores propietarios específicos. |
| **AWS SQS** | Cola de Mensajería Cloud Gestionada | Desacoplar microservicios en la nube sin administrar infraestructura (ej. microservicio de ingesta envía a cola -> microservicio de OCR procesa). | Streaming continuo con ordenamiento estricto a gran escala y repetición histórica de eventos. |

---

## 7. Librerías Python Clave

| Librería | Propósito | ✅ Cuándo USARLA | ❌ Cuándo NO USARLA |
| :--- | :--- | :--- | :--- |
| **Pydantic (v2)** | Validación y Serialización | Validar tipos en runtime, schemas de APIs FastAPI y parsear variables de entorno seguras (`pydantic-settings`). | Manipulación matemática intensiva de vectores (usa NumPy). |
| **SQLAlchemy (2.0)** | ORM & Query Builder | Mapear objetos Python a tablas relacionales de forma tipada, segura contra inyecciones SQL y con soporte asíncrono (`asyncpg`). | Consultas OLAP de agregación masiva sobre millones de filas (usa DuckDB). |
| **Pandas** | Análisis y Limpieza Tabular | Exploración de datos (EDA), limpieza, cruces y filtrado de datasets que caben en memoria RAM (< 5 GB). | Datasets gigantescos (> 10 GB) que saturan la RAM o consultas analíticas directas sobre Parquet (usa DuckDB o Spark). |
| **NumPy** | Cálculo Numérico & Matrices | Operaciones algebraicas vectorizadas, similitud de coseno, normalización L2 y serialización rápida con `.npz`. | Tablas con columnas de texto heterogéneas y fechas (usa Pandas). |
| **HTTPX** | Cliente HTTP Async | Hacer peticiones HTTP/2 asíncronas concurrentes a APIs externas o LLMs dentro de corrutinas FastAPI. | Scripts sincrónicos de un solo hilo donde `requests` es suficiente y más simple. |
| **APScheduler** | Programador de Tareas | Ejecutar tareas periódicas (crons, chequeo de churn cada 10 min) dentro del mismo proceso del backend. | Tareas pesadas distribuidas que requieren reintentos en múltiples servidores (usa Celery o Airflow). |
| **Playwright** | Browser Automation & Scraping | Scrapear aplicaciones modernas con renderizado dinámico en JavaScript, login complejo o capturas visuales. | Páginas HTML estáticas simples donde `BeautifulSoup` + `requests` es 20x más rápido y no gasta memoria de navegador. |
| **PyPDF / PyMuPDF (fitz)** | Lectura & Extracción de PDFs | Extraer texto estructurado y metadatos de documentos para alimentar pipelines de RAG y LLMs. | Documentos escaneados donde el texto es una imagen (requiere OCR como RapidOCR o Tesseract). |
| **WeasyPrint** | Generador de PDFs | Compilar contratos y reportes PDF con diseño editorial profesional usando HTML5 y CSS3 moderno. | Edición o manipulación de PDFs ya existentes (usa PyPDF). |
| **Faker (es_ES)** | Datos Sintéticos | Generar datasets de prueba realistas (nombres, cédulas, correos, fechas, contratos) para tests de carga. | Generación de texto semántico contextual (usa LLMs). |
| **RapidOCR / EasyOCR** | Reconocimiento Óptico de Caracteres | Extraer texto de imágenes, facturas o recibos escaneados con soporte local ONNX sin costo por API. | PDFs nativos con texto seleccionable embebido (usa PyPDF). |

---

## 8. Librerías JavaScript & TypeScript Clave

| Librería | Propósito | ✅ Cuándo USARLA | ❌ Cuándo NO USARLA |
| :--- | :--- | :--- | :--- |
| **Vite** | Bundler & Dev Server | Iniciar proyectos React, Vue o Vanilla JS con recarga ultrarrápida (HMR) gracias a ES Modules nativos. | Proyectos heredados con configuraciones monolíticas cerradas de Webpack. |
| **Lucide React** | Iconografía | Iconos limpios, consistentes, tree-shakeable y personalizables para interfaces modernas. | Iconos animados complejos de alta fidelidad (usa Lottie). |
| **Framer Motion** | Animaciones React | Animaciones fluidas basadas en física, micro-interacciones y transiciones de entrada/salida de componentes. | Páginas con requerimientos extremos de rendimiento donde CSS Transitions nativas bastan. |
| **Chart.js** | Visualización 2D | Gráficos reactivos limpios de líneas, barras y donas para dashboards analíticos en Canvas. | Gráficos 3D inmersivos (usa Three.js) o visualizaciones científicas hiper-personalizadas (usa D3.js). |
| **Concurrently** | Utilidad CLI | Ejecutar varios comandos en paralelo en un solo comando terminal (ej. `npm run dev` para front y back juntos). | Tareas de producción que deben gestionarse con Docker o PM2. |
| **JSON-Server** | Mock REST API | Simular un backend REST completo con un solo archivo `db.json` para pruebas de frontend inmediatas. | Entornos de producción o pruebas de estrés con transacciones concurrentes. |

---

## 9. DevOps, Testing & Herramientas

| Herramienta | Propósito | ✅ Cuándo USARLA | ❌ Cuándo NO USARLA |
| :--- | :--- | :--- | :--- |
| **Docker & Docker Compose** | Contenedores & Orquestación Local | Asegurar reproducibilidad ("funciona en mi máquina y en el servidor"), aislando bases de datos, APIs y workers. | Scripts locales sencillos que no tienen dependencias externas ni bases de datos. |
| **uv** | Package Manager (Rust) | Instalar y resolver paquetes Python en milisegundos (10x-100x más rápido que pip). | Ambientes con sistemas operativos muy antiguos o sin soporte de binarios de Rust. |
| **Pytest & Pytest-Asyncio** | Testing Unitario e Integración | Escribir tests limpios con fixtures, parametrización y soporte nativo para corrutinas asíncronas en FastAPI. | Pruebas de estrés masivo de carga de red (usa **k6** o **Locust**). |
| **Vitest** | Testing Frontend | Ejecutar pruebas unitarias para React y TypeScript con la misma configuración de Vite y altísima velocidad. | Proyectos basados en Jest que no usan Vite. |
| **OpenMetadata** | Gobernanza de Datos | Catálogo centralizado de datos, linaje automático (quién consume qué tabla), reglas de calidad y glosario de términos. | Pequeñas bases de datos de una sola aplicación sin un equipo de datos compartido. |

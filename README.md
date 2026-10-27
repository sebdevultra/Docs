# 📚 Centro de Documentación Técnica - Proyectos de Sebastian Ortiz

> **Propósito:** Guía de referencia rápida, inventario tecnológico y glosario para consulta inmediata durante pruebas técnicas, entrevistas de código y diseño de sistemas.  
> **Ubicación:** `Desktop/Documentacion`  
> **Fecha de Compilación:** Octubre 2026  
> **Proyectos Analizados:** 20+ proyectos en `Desktop` (Data Engineering, AI Engineering, Full-Stack, Microservicios, Event-Driven, OCR, Scraping y RAG).

---

## 🧭 Mapa de Navegación Rápida

| Archivo | Contenido Clave | ¿Cuándo Consultarlo en la Prueba? |
| :--- | :--- | :--- |
| [**01_GLOSARIO_MAESTRO_CUANDO_USAR_Y_CUANDO_NO.md**](file:///C:/Users/SEBAS/Desktop/Documentacion/01_GLOSARIO_MAESTRO_CUANDO_USAR_Y_CUANDO_NO.md) | **El recurso principal.** Qué es cada herramienta, cuándo usarla, cuándo NO usarla y alternativa recomendada. | Si te preguntan: *"¿Por qué elegirías X sobre Y?"* o *"¿Cuándo no deberías usar esta tecnología?"* |
| [**02_FRAMEWORKS.md**](file:///C:/Users/SEBAS/Desktop/Documentacion/02_FRAMEWORKS.md) | FastAPI, Express.js, React, Tailwind CSS, Apache Airflow, Apache Spark, Streamlit, Three.js. | Preguntas sobre arquitectura de backend, frontend, procesamiento distribuido u orquestación de datos. |
| [**03_LIBRERIAS_PYTHON.md**](file:///C:/Users/SEBAS/Desktop/Documentacion/03_LIBRERIAS_PYTHON.md) | Pydantic v2, SQLAlchemy 2.0, DuckDB, Pandas, NumPy, HTTPX, PyPDF, WeasyPrint, Faker, Playwright, RapidOCR, etc. | Preguntas de código Python, manipulación de datos, validación, APIs asíncronas, PDFs o scraping. |
| [**04_LIBRERIAS_JS_TS.md**](file:///C:/Users/SEBAS/Desktop/Documentacion/04_LIBRERIAS_JS_TS.md) | Vite, Lucide-React, Framer Motion, Chart.js, Concurrently, PG, UUID, JSON-Server, Vitest. | Preguntas sobre desarrollo Web, SPAs en JavaScript, TypeScript, componentes UI o mocks para frontend. |
| [**05_IA_LLMS_AGENTES_Y_RAG.md**](file:///C:/Users/SEBAS/Desktop/Documentacion/05_IA_LLMS_AGENTES_Y_RAG.md) | Google GenAI SDK (Gemini 2.5/3.5), OpenAI SDK, LangChain, LangGraph, ChromaDB, Qdrant, Tiktoken, PyTorch, n8n. | Preguntas sobre Inteligencia Artificial, RAG, Embeddings, Flujos de Agentes Autónomos y optimización de tokens. |
| [**06_BASES_DE_DATOS_STORAGE_Y_STREAMING.md**](file:///C:/Users/SEBAS/Desktop/Documentacion/06_BASES_DE_DATOS_STORAGE_Y_STREAMING.md) | PostgreSQL, SQLite, DuckDB, MinIO (S3), Redis, Apache Kafka, AWS SQS, ClickHouse, OpenMetadata. | Preguntas sobre persistencia OLTP vs OLAP, colas de mensajería, cachés, data lakehouse y gobernanza de datos. |
| [**07_DEVOPS_TESTING_Y_ARQUITECTURA.md**](file:///C:/Users/SEBAS/Desktop/Documentacion/07_DEVOPS_TESTING_Y_ARQUITECTURA.md) | Docker, Docker Compose, Pytest (Asyncio/Cov), Vitest, uv, Medallion Architecture, Microservicios, Clean Architecture. | Preguntas sobre testing, despliegue, CI/CD, diseño de sistemas y justificación de patrones arquitectónicos. |
| [**08_MAPA_DE_PROYECTOS.md**](file:///C:/Users/SEBAS/Desktop/Documentacion/08_MAPA_DE_PROYECTOS.md) | Inventario exacto de carpetas de proyectos en tu Desktop, stack utilizado y funcionalidades clave. | Para dar ejemplos reales de tu propia experiencia: *"En mi proyecto Pipeline-analitica implementé..."* |

---

## ⚡ Cheat Sheet Mental para la Prueba

### 1. Las 4 Preguntas Clásicas de Justificación Técnica
Ante cualquier pregunta de arquitectura o selección tecnológica, responde siguiendo esta estructura:
1. **El Problema:** *"Necesitamos resolver X (baja latencia, consistencia transaccional, procesamiento masivo, etc.)."*
2. **La Elección:** *"Para esto selecciono [Tecnología] porque ofrece [ventaja técnica concreta]."*
3. **El Trade-off (Compromiso):** *"El trade-off es [ej. mayor consumo de RAM, eventual consistency, curva de aprendizaje], el cual mitigamos mediante [ej. índices, particionamiento, cachés]."*
4. **La Alternativa Rechazada:** *"No usé [Alternativa] porque [razón técnica por la que sería sobreingeniería o ineficiente]."*

### 2. Regla de Oro OLTP vs OLAP
* **OLTP (Online Transaction Processing):** Transacciones individuales rápidas, `INSERT/UPDATE/DELETE` por ID, atomicidad ACID.
  * *Herramientas:* **PostgreSQL**, **SQLite**, **MySQL**.
* **OLAP (Online Analytical Processing):** Consultas analíticas pesadas, agregaciones (`SUM`, `AVG`, `GROUP BY`) sobre millones de registros, almacenamiento columnar.
  * *Herramientas:* **DuckDB** (in-process / local), **ClickHouse** (servidor distribuido), **Apache Spark** (Big Data distribuido).

### 3. Regla de Oro en IA & LLMs
* **¿Cuándo usar LLM Cloud (Gemini / OpenAI)?** Tareas que requieren razonamiento complejo, síntesis abierta, ambigüedad o generación creativa.
* **¿Cuándo usar Embeddings / NLP Local (Sentence-Transformers / PyTorch)?** Búsqueda semántica, similitud de texto, clasificación de alta frecuencia y privacidad total sin costo por token.
* **¿Cuándo usar Reglas / Regex / RapidFuzz?** Validación de formato, detección de keywords, filtrado de primer nivel (< 1ms, 0 costo, determinista).

# N8N_Practice_IIA
Ejercicio de clase planteado como 3er trabajo de la asignatura Introducción a la IA de la maestria de ciencia de datos y analitica en EAFIT
Ejercicio desarrollado por Juan David Marin y Juan Manuel Peña 

# 📂 Automatización de Descarga, Resumen y Distribución de Artículos de arXiv con n8n

## 🚀 Descripción del Proyecto
Este proyecto implementa un **agente automatizado en n8n** que realiza la búsqueda, descarga, organización, resumen y distribución (via correo) de artículos académicos provenientes de **arXiv**.  
El flujo está diseñado para reducir tareas manuales repetitivas, garantizar la consistencia de formatos y facilitar la adquisición de conocimiento a través de correo electrónico.

---

## 🎯 Problema que buscamos resolver
- **Automatizar la ejecución inicial de investigación**: obtener y resumir artículos relevantes de un tema especifico sin intervención manual.  
- **Ahorrar tiempo y evitar errores**: eliminar procesos manuales de descarga, organización y envío.  
- **Centralizar la información**: almacenar documentos y resúmenes en carpetas organizadas en Google Drive.  
- **Estandarizar entregables**: resúmenes uniformes, listos para consulta y uso.  
- **Agilizar la toma de decisiones**: entregar contenido procesado directamente a los interesados vía correo electrónico.  

---

## 🛠️ ¿Por qué n8n?
- **Ecosistema visual y modular**: permite encadenar tareas (cron → API arXiv → Google Drive → LLM → Email) de forma clara y explicable sin necesidad de código avanzado.  
- **Integraciones listas + personalización**: cuenta con conectores dentro de la misma plataforma (Drive, Gmail, HTTP) combinados con bloques de código de preferencia del usuario, pero puede realizarse bastantte trabajo sin necesidad de escribir código.  
- **Automatización basada en disparadores**: ejecución programada con disparadores que pueden ser basadas en tiempo o sucesos especificos.  
- **Gestión segura de credenciales**: almacenamiento centralizado de llaves y accesos.  
- **Resiliencia operativa**: manejo de errores, reintentos automáticos y logs por nodo.  
- **Escalabilidad y costo eficiente**: auto-hosteable, flexible y más económico que SaaS cerrados, el costo realmente va a depender de la tarea especifica y no de la herrmaienta a usar, por lo tanto es más viable para proyectos que usen pocas API´s  
- **Extensibilidad futura**: se puede cambiar las fuentes de descarga de los archivos académicos como también del servicio en que se almacene dicha información obtenida.
---

## 🔄 Flujo de Trabajo
1. ** Disparador basado en tiempo (Cron):** inicia el flujo a la hora programada.  
2. ** Creación de carpeta en Google Drive:** organiza los documentos de la ejecución.  
3. ** Consulta a arXiv:** búsqueda y descarga de artículos específicos.  
4. ** Procesamiento inicial (JavaScript):** normalización de formato y nombres.  
5. ** División en dos ramas:**  
   - **Almacenamiento de artículos** en la carpeta de Google Drive.  
   - **Procesamiento con LLM** para generar resúmenes de cada texto.  
6. ** Postprocesamiento (JavaScript):** organización de resúmenes en formato estandarizado.  
7. ** Guardado de resúmenes en Drive:** en la carpeta creada en el paso 2.  
8. ** Envío por correo electrónico:** distribución automática de los resúmenes a los destinatarios definidos.  

---

## 📊 Beneficios clave
-  Reducción de horas de trabajo manual.  
-  Organización consistente y trazable en Google Drive.  
-  Distribución inmediata de resultados procesados.  
-  Fácil mantenimiento y escalabilidad del flujo.  





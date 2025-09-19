# N8N_Practice_IIA
Ejercicio de clase planteado como 3er trabajo de la asignatura Introducción a la IA de la maestria de ciencia de datos y analitica en EAFIT
Ejercicio desarrollado por Juan David Marin y Juan Manuel Peña 

# 📂 Automatización de Descarga, Resumen y Distribución de Artículos de arXiv con n8n

##  Descripción del Proyecto
Este proyecto implementa un **agente automatizado en n8n** que realiza la búsqueda, descarga, organización, resumen y distribución (via correo) de artículos académicos provenientes de **arXiv**.  
El flujo está diseñado para reducir tareas manuales repetitivas, garantizar la consistencia de formatos y facilitar la adquisición de conocimiento a través de correo electrónico.

El flujo esperado es: 
Consulta arXiv (LLM/Agents) →  Guarda PDFs en Drive →  Resume con Gemini →  Comparte resúmenes automáticamente.

---

##  Problema que buscamos resolver

El proyecto nace de la necesidad de automatizar la ejecución inicial de investigación, en este caso la consulta de artículos académicos en arXiv sobre temas específicos (En este caso LLM OR AGENTS). Tradicionalmente, este proceso requiere buscar manualmente los documentos, descargarlos, organizarlos y, posteriormente, generar resúmenes para su lectura con herramientas como LM Notebook u otras IA´s generativas o Modelos de lenguaje. 
Al implementar este flujo, se logra ahorrar tiempo y evitar errores al eliminar por completo la gestión manual. Además, se consigue centralizar la información al almacenar tanto los documentos como sus resúmenes en carpetas organizadas dentro de Google Drive, lo que facilita la trazabilidad y el acceso compartido.
Otro beneficio clave es la estandarización de los entregables, ya que los resúmenes generados por el agente siguen un formato uniforme, listo para consulta y uso inmediato. Finalmente, este proceso automatizado permite agilizar la toma de decisiones, dado que los resúmenes procesados se entregan directamente a los interesados por correo electrónico, reduciendo la brecha entre la publicación de un artículo y su aprovechamiento práctico.

---

##  ¿Por qué n8n?

La elección de n8n responde a su capacidad de funcionar como un ecosistema visual y modular, que permite organizar tareas complejas sin necesidad de escribir código avanzado.
La plataforma ofrece integraciones listas con opción de personalización, ya que dispone de conectores nativos como Google Drive, Gmail y otros servicios, al tiempo que admite bloques de código cuando se requieren ajustes específicos sin limitarse a un lenguaje especifico. Esto permite equilibrar simplicidad con flexibilidad.
Otro aspecto fundamental es la automatización basada en disparadores, que puede configurarse tanto por intervalos de tiempo como por eventos específicos. Además, n8n garantiza una gestión segura de credenciales, al centralizar llaves y accesos, y aporta resiliencia operativa gracias a su manejo de errores, reintentos automáticos y la trazabilidad mediante logs por nodo.
Finalmente, destaca por su escalabilidad y costo eficiente, el costo real se ajusta al volumen y tipo de tareas, lo cual lo hace viable incluso en proyectos pequeños que solo consumen pocas APIs. A esto se suma su extensibilidad futura, dado que es posible modificar fácilmente las fuentes de descarga de los artículos o cambiar el servicio de almacenamiento de la información procesada.

##  Flujo de Trabajo
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

##  Beneficios clave
-  Reducción de horas de trabajo manual.  
-  Organización consistente y trazable en Google Drive.  
-  Distribución inmediata de resultados procesados.  
-  Fácil mantenimiento y escalabilidad del flujo.  


#  Guía de Uso (My workflow.json)

Esta guía te explica cómo poner a funcionar el flujo `My workflow.json` en n8n para descargar artículos de arXiv, guardarlos en Google Drive, generar resúmenes con **Gemini**, y compartirlos automáticamente por correo.

---

##  Requisitos previos

1. **Instalar n8n**  
   - Opción rápida: Usar n8n desktop o desde la web.  
   - Opción avanzada: instalación en servidor o Docker.  

2. **Credenciales necesarias en n8n**  
   - **Google Drive (OAuth2)** → para crear carpetas y subir archivos.  
   - **Gemini API Key** → desde [Google AI Studio], de forma gratuita).  

3. **Haber descargado previamente el archivo de workflow adjunto en el repo 'My workflow.json'**.  

---
# Disclaimer: Toda esta guia está pensada para la versión web de n8n.

## Configuración inicial

1. **Configurar credenciales**  
   - En el menú superior del proyecto dentro de n8n en la pestaña de tu proyecto → *Credentials*.  
   - Añade:
     - Google Drive OAuth2  
     - Gemini API Key
     - No requerimos una API Key de ArXiv dado que se ejecuta un http request

2. **Importar el flujo**  
   - Abre n8n → Parte superior derecha → *Import from File*.  
   - Carga `My workflow.json`.  
   - Verás un flujo con varios nodos (Trigger, HTTP Request, XML, Code, Google Drive, Gemini, etc.).  

3. **Revisar parámetros principales**  

   - **Trigger**  
     - El flujo está configurado para dispararse todos los días a las **08:00 AM**.  
     - Puede cambiarse en el nodo **Schedule Trigger** si se necesita otro horario.  

   - **Consulta a arXiv**  
     - El nodo **HTTP Request** ya está configurado con la query:  
       ```
       https://export.arxiv.org/api/query?search_query=all:("large language model" OR "Agents")&start=0&max_results=5&sortBy=submittedDate&sortOrder=descending
       ```
     - Eso significa que **trae los 5 artículos más recientes de arXiv sobre LLM o Agents**.  
     - Si se requiere cambiar de tema:  
       - Reemplaza `"large language model" OR "Agents"` por tus otras clave.  
     - Si requieres más artículos: cambia `max_results=5` por otro número.  

   - **Carpetas en Google Drive**  
     - Los PDFs se guardan en la carpeta `papers`.  
     - Los resúmenes `.txt` se guardan en la carpeta `Summary`.  

   - **Compartir carpeta**  
     - Actualmente el flujo comparte la carpeta `Summary` con el correo que se especifique dentro del nodo.
     - Cámbialo en el nodo **Share folder** si quieres otros destinatarios.  

---

##  Ejecución

- **Automática:** se ejecuta cada día a las **08:00 AM**.  
- **Manual:** abre el flujo en n8n y haz clic en **Execute Workflow**.  

---

##  Resultados esperados

1. Se crea una carpeta `papers` en Google Drive con los **PDFs de arXiv**.  
2. Se crea una carpeta `Summary` con los **resúmenes en formato Markdown** generados por Gemini.  
3. La carpeta de resúmenes se comparte automáticamente con el correo configurado.  

---

##  Posibles problemas 

- **No se crea la carpeta en Drive** → revisa las credenciales de Google Drive.  
- **No se generan resúmenes** → confirma que la API Key de Gemini esté activa.  
- **Resúmenes vacíos** → revisa el prompt en el nodo *Analyze document* y ajusta tokens.  
- **No llega el correo** → revisa el nodo *Share folder* y la dirección configurada.  


# ⚡ Fuentes de Datos

<p class="tip">
    <strong>Aviso Importante:</strong>
    <br>
    Este módulo contiene información técnica. Si encuentras términos desconocidos como <em>modelo semántico</em>, <em>origen de datos</em> o <em>API</em>, te recomendamos reforzar estos conceptos para aprovechar mejor la información presentada.
</p>

## 🗂️ Origen de los Datos

El tablero de recepciones se alimenta de datos extraídos de la base de datos de IP6 mediante una tarea programada en el servidor. Este proceso sigue los siguientes pasos:

1- Extracción de Datos: La tarea realiza solicitudes HTTP a una API específica para obtener la información requerida.  

2- Generación del Archivo JSON: Los datos obtenidos se guardan en un archivo JSON que se sobrescribe automáticamente cada 5 minutos. Este archivo se almacena en: **"Documentos/01. Proyectos/05. PIA/Data Sources"** del OneDrive de la cuenta: jose.maestre@logismart.com.co  

3- Actualización en Power BI Service: 
- El conjunto de datos en Power BI Service se actualiza automáticamente cada 30 minutos.  
- Este proceso se ejecuta desde las 6:00 a.m. hasta las 8:00 p.m., utilizando el archivo JSON almacenado en OneDrive.  
- Durante la actualización, los datos se transforman y preparan para su visualización en el tablero.  

La API utilizada para este proceso es:
**http://129.146.161.23/api/BI/reportes_pia.php?recepcion**

---

## 🧩 Modelo Semántico

**¿Qué es un modelo semántico en Power BI?**

Es una capa lógica que organiza y gestiona las relaciones, transformaciones y cálculos necesarios entre las fuentes de datos para crear paneles e informes claros y funcionales.

En el caso del tablero de recepciones, el modelo semántico se ha diseñado específicamente para procesar y visualizar los datos relacionados con el proceso de recepción.

---

### Características Principales

**1- Transformaciones Internas:** Todas las transformaciones de los datos se realizan dentro de Power Query, no en la fuente original. Este enfoque garantiza que los datos sean:

• Limpios.  
• Estructurados.  
• Tipificados adecuadamente.  

Esto asegura la calidad y precisión de la información presentada en el tablero, evitando modificaciones externas a los datos originales.

**2- Optimización del Rendimiento:** El modelo semántico está estructurado para ofrecer una experiencia ágil y eficiente al interactuar con el tablero. 

A continuación te presentamos cómo están relacionadas las diferentes consultas del modelo:

<img src="http://129.146.151.214:8080/api/BI/_asset/01_Recepcion/013-modelo_datos.png" alt="Modelo de Datos - PIA Recepción" loading="lazy"/>  

---

## 🎉 ¡Módulo Completado!

¡Felicidades! Ahora comprendes cómo el tablero de recepciones obtiene, transforma y organiza los datos desde su origen hasta la visualización final. En los siguientes módulos, exploraremos las medidas DAX y fórmulas para maximizar tus conocimientos.
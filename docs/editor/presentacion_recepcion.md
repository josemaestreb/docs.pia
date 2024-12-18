# 🎯 Navegando Por El Tablero

---
Para acceder al tablero, utiliza el siguiente enlace: <a href="https://josemaestreb.github.io/01_Pharex_Workspace/Tablero%20PIA%20-%20Recepciones.html" target="_blank"><strong>Acceso aquí</strong></a>

¡Enhorabuena! Si accediste correctamente, deberías ver una pantalla similar a la siguiente:  

<img src="https://josemaestreb.github.io/docs.pia/_asset/01_Recepcion/001-presentacion.png" alt="Pantalla inicial de PIA Recepción" loading="lazy"/>  

En esta vista inicial, notarás que el tablero está compuesto por cuatro secciones clave:

**1- Grado:** Controla los niveles de visualización de datos.  
**2- Resumen:** Ofrece un panorama general de la operación.  
**3- Cuadro de Búsqueda:** Filtra datos por producto o artículo.  
**4- Indicadores Principales:** Presenta datos clave de ingresos y filtros avanzados.  

<p class="tip"><strong>Nota Importante: </strong>Este tablero muestra únicamente recepciones finalizadas (estado 3000 o superior) registradas en IP6. Las recepciones en estado 100, o pendientes, están excluidas, y no se reflejan en los datos. Asegúrate de validar correctamente las entradas, en el nuevo monitor de recepciones, para garantizar que todas las recepciones se registren adecuadamente.</p>

---

## 🧩 Secciones Principales
A continuación, analizaremos cada una de estas secciones para que puedas entender cómo aprovecharlas al máximo:  

### 1. Grado
Ubicada en la parte superior, esta sección permite alternar entre diferentes niveles de visualización de datos, como:  

<img src="https://josemaestreb.github.io/docs.pia/_asset/01_Recepcion/009-grado.png" alt="Grado Fechas de PIA Recepción" loading="lazy"/>  

- **Mensual:** Visualiza valores mes a mes (enero, febrero, etc.).
- **Diario Semana:** Muestra datos agrupados por día de la semana (lunes a domingo).
- **Diario Mes:** Presenta datos por día del mes, independientemente del mes (01 al 30).
- **Histórico:** Exhibe valores acumulados desde el inicio del registro de datos hasta la fecha más reciente.

---

### 2. Resumen
Esta sección proporciona un panorama general de la operación de recepción desde el inicio del registro de datos hasta la fecha actual. Incluye:

<img src="https://josemaestreb.github.io/docs.pia/_asset/01_Recepcion/002-resumen.png" alt="Resumen de PIA Recepción" loading="lazy"/>  

- **Ingresos Unidades:** Total acumulado de unidades recepcionadas.
- **Ingresos Cajas:** Total acumulado de cajas recepcionadas.
- **Ingresos Líneas:** Total acumulado de líneas recepcionadas.
- **Ingreso por producto:** Total acumulado de unidades recepcionadas por producto o artículo.

---

### 3. Cuadro de Búsqueda
Permite filtrar información escribiendo el nombre del producto o artículo en el recuadro de la derecha.

<img src="https://josemaestreb.github.io/docs.pia/_asset/01_Recepcion/008-producto.png" alt="Cuadro de Búsqueda de PIA Recepción" loading="lazy"/>  

---

### 📣 Información Importante

Antes de explorar los indicadores principales del tablero, queremos que tengas en cuenta lo siguiente: 

Por defecto, los ingresos por producto y el indicador de "Ingresos" se muestran en unidades. Sin embargo, si lo prefieres, puedes cambiar esta visualización para verlos en cajas o líneas.

Solo necesitas ajustar los **filtros del tablero**, y más adelante te explicaremos cómo hacerlo paso a paso.

<img src="https://josemaestreb.github.io/docs.pia/_asset/01_Recepcion/003-tooltip.gif" alt="Tooltip PIA Recepción" loading="lazy"/>  

<p class="tip">Al posicionarte sobre indicadores o gráficos, se activa la funcionalidad <strong>Tooltip</strong>, que despliega información adicional no visible a simple vista. Ilustración arriba.</p>

---

### 4. Indicadores Principales

#### A) Ingresos
Este indicador muestra la cantidad de unidades, cajas o líneas en el tiempo, permitiendo analizar tendencias y patrones estacionales.

<img src="https://josemaestreb.github.io/docs.pia/_asset/01_Recepcion/004-ingresos.png" alt="Ingresos PIA Recepción" loading="lazy"/>  

---

#### B) On Time: Recepción e Inspección
Estos KPIs miden el cumplimiento en los tiempos establecidos para recepción e inspección de mercancías, en términos porcentuales.  

<img src="https://josemaestreb.github.io/docs.pia/_asset/01_Recepcion/005-ontime.png" alt="On Time PIA Recepción" loading="lazy"/>  

- **Recepción:** Calcula los días hábiles entre la fecha de recepción y el envío del reporte logístico. Se considera cumplido si el tiempo es de 2 días hábiles o menos.

- **Inspección:** Calcula los días hábiles entre el envío del reporte logístico y el envío del parte de calidad. Se cumple si el tiempo es de 1 día hábil o menos.

---

#### C) Cumplimiento y Promedio de Días de Recepción
Estos indicadores permiten analizar:  

- Promedio de días de recepción (a la izquierda en la imagen).

- Cumplimiento en los tiempos establecidos para recepción a lo largo del tiempo, desglosado por meses, días e histórico (a la derecha en la imagen).

<img src="https://josemaestreb.github.io/docs.pia/_asset/01_Recepcion/006-dias_rec.png" alt="Promedio Días PIA Recepción" loading="lazy"/>  

---

#### D) Cumplimiento Inspección
Este indicador evalúa el cumplimiento en los tiempos establecidos para la inspección, analizado a lo largo del tiempo, con detalles por meses, días e histórico.

<img src="https://josemaestreb.github.io/docs.pia/_asset/01_Recepcion/007-cumplimiento_insp.png" alt="Inspección PIA Recepción" loading="lazy"/> 

<p class="tip">Los últimos tres indicadores (B, C y D) están actualmente en <strong>stand-by</strong>, ya que su funcionamiento depende de la fecha de envío del reporte logístico y del parte de calidad, los cuales se generan automáticamente a través de los correos configurados por PHAREX. Por este motivo, en su estado actual, los indicadores emplean datos de prueba y no reflejan información real.</p>

---

## Otras Funcionalidades

¡Excelente! Si has llegado hasta aquí, ya estás listo para interactuar con el tablero de manera segura y eficiente. A continuación, te presentamos unas últimas funciones clave para aprovechar al máximo la herramienta.

---

### • Menú de Filtros

Para abrir el menú de filtros, simplemente haz clic en el botón indicado en la siguiente imagen:

<img src="https://josemaestreb.github.io/docs.pia/_asset/01_Recepcion/011-abrir_filtro.png" alt="Abrir Filtros PIA Recepción" loading="lazy"/> 

Este menú te permite personalizar la visualización de los datos. Como ejemplo, te mostramos cómo:

**1-** Aplicar un filtro por usuario de recepción.  
**2-** Cambiar la unidad de medida a "Líneas".  
**3-** Restablecer todos los filtros y cerrar el menú.  

<img src="https://josemaestreb.github.io/docs.pia/_asset/01_Recepcion/010-filtros.gif" alt="Filtros PIA Recepción" loading="lazy"/> 

<p class="tip">En este menú de filtros, puedes cambiar cómo se muestran los ingresos o recepciones eligiendo entre Cajas, Líneas o Unidades como unidad de medida. Solo necesitas ajustar la opción en <strong>"Tipo Medida"</strong>.</p>

---

### • Obtén Información Adicional

Si necesitas recordar rápidamente el significado de un indicador, consultar información específica o reforzar lo aprendido, puedes hacerlo fácilmente desde el menú de filtros. Solo sigue estos pasos para acceder a información detallada directamente desde el tablero:

<img src="https://josemaestreb.github.io/docs.pia/_asset/01_Recepcion/012-info_adicional.gif" alt="Filtros PIA Recepción" loading="lazy"/>  

---

## 🎉 ¡Módulo Completado!

¡Enhorabuena! Ahora cuentas con un entendimiento completo del tablero de recepción. En los próximos módulos, profundizaremos en las fuentes de datos, medidas DAX, fórmulas y más herramientas para maximizar tu análisis.
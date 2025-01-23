# 🎯 Navegando Por El Tablero

<!-- Ruta para assets en producción (GitHub): "https://josemaestreb.github.io/docs.pia/_asset/03_transporte/"  -->

---
Para acceder al tablero, utiliza el siguiente enlace: <a href="http://129.146.161.23/portal_pharex/operative/Tablero-PIA-Transporte.html" target="_blank"><strong>Acceso aquí</strong></a>

¡Enhorabuena! Si accediste correctamente, deberías ver una pantalla similar a la siguiente:  

<img src="https://josemaestreb.github.io/docs.pia/_asset/03_transporte/00-dashboard.png" alt="Pantalla inicial de PIA Transporte" loading="lazy"/>  

En esta vista inicial, notarás que el tablero está compuesto por cuatro secciones clave:

**1- Grado:** Controla los niveles de visualización de datos.  
**2- Resumen:** Ofrece un panorama general de la operación.  
**3- Cuadro de Búsqueda:** Filtra datos por destinatario o cliente final.  
**4- Indicadores Principales:** Presenta datos clave como volumen, cumplimiento del On Time y filtros avanzados.  

---

## 🧩 Secciones Principales
A continuación, analizaremos cada una de estas secciones para que puedas entender cómo aprovecharlas al máximo:  

### 1. Grado
Ubicada en la parte superior, esta sección permite alternar entre diferentes niveles de visualización de datos, como:  

<img src="https://josemaestreb.github.io/docs.pia/_asset/03_transporte/01-grado.png" alt="Grado Fechas de PIA Transporte" loading="lazy"/>  

- **Mensual:** Visualiza valores mes a mes (enero, febrero, etc.).
- **Diario Mes:** Presenta datos por día del mes, independientemente del mes (01 al 30).
- **Diario Semana:** Muestra datos agrupados por día de la semana (lunes a domingo).
- **Histórico:** Exhibe valores acumulados desde el inicio del registro de datos hasta la fecha más reciente.

---

### 2. Resumen
Proporciona un panorama general de la operación de Transporte desde el inicio del registro hasta la fecha actual. Incluye:  

<img src="https://josemaestreb.github.io/docs.pia/_asset/03_transporte/01-resumen.png" alt="Resumen de PIA Transporte" loading="lazy"/>  

- **On Time:** Porcentaje promedio de pedidos entregados a tiempo.
- **In Full:** Porcentaje promedio de pedidos entregados completos y sin defectos.
- **OTIF:** Porcentaje promedio de pedidos entregados de manera perfecta.
- **OTIF Pharex:** Porcentaje promedio de pedidos que NO se vieron afectados por responsabilidad de PHAREX.
- **Tipo Destino:** Acumulado de pedidos según tipo destino (Local o Nacional).
- **Top Destinatarios:** Top 10 destinatario con mayor acumulado de unidades, pedidos, etc.

---

### 3. Cuadro de Búsqueda
Esta funcionalidad permite filtrar información escribiendo el nombre del destinatario o cliente final en el recuadro de la derecha.

<img src="https://josemaestreb.github.io/docs.pia/_asset/03_transporte/02-destinatario.png" alt="Cuadro de Búsqueda de PIA Transporte" loading="lazy"/>  

---

### 📣 Información Importante

Antes de explorar los indicadores clave del tablero, considera lo siguiente:

Por defecto, todos los indicadores, a excepción de **"Top Destinatarios On Time / In Full / OTIF"** se muestran en unidades. Sin embargo, puedes cambiar la visualización para verlos en cajas o pedidos, si así lo prefieres.

Para realizar este cambio, simplemente **ajusta los filtros del tablero**. Más adelante te explicaremos el procedimiento paso a paso.

<img src="https://josemaestreb.github.io/docs.pia/_asset/03_transporte/03-tooltip.gif" alt="Tooltip PIA Transporte" loading="lazy"/>  

<p class="tip">Al posicionarte sobre indicadores o gráficos, se activa la funcionalidad <strong>Tooltip</strong>, que despliega información adicional no visible a simple vista. Ilustración arriba.</p>

---

### 4. Indicadores Principales

#### A) Cantidad de Unidades / Pedidos / Cajas
Muestra la cantidad de unidades, cajas o pedidos servidos durante el período, dependiendo de la variable de medición seleccionada, permitiendo analizar tendencias y patrones estacionales.  

<img src="https://josemaestreb.github.io/docs.pia/_asset/03_transporte/04-cantidad.png" alt="Cantidad PIA Transporte" loading="lazy"/>  

---

#### B) Comportamiento del On Time / In Full / OTIF
Gráfico de líneas que muestra el cumplimiento de los tiempos de entrega establecidos (On Time), la precisión en entregas perfectas (In Full) o ambos indicadores (OTIF), según la variable seleccionada. Los datos se presentan en porcentajes a lo largo del tiempo. Las reglas y cálculos correspondientes se detallarán en el siguiente módulo.  

<img src="https://josemaestreb.github.io/docs.pia/_asset/03_transporte/05-on-time.png" alt="OnTime PIA Transporte" loading="lazy"/>  

---

#### C) Detalle Cantidad y Cumplimiento por Ciudad
Tabla que permite visualizar el porcentaje del total unidades, pedidos o cajas, junto con los cumplimientos previos desglosados por ciudad o destino.  

<img src="https://josemaestreb.github.io/docs.pia/_asset/03_transporte/06-detalle-ciudad.png" alt="Detalle Cantidad por Ciudad - PIA Transporte" loading="lazy"/>  

---

#### D) Top Destinatarios On Time / In Full / OTIF
Gráfico de barras que visualiza los destinatarios con más pedidos afectados (Top 10) en On Time, In Full o OTIF, clasificados por responsable.  

<img src="https://josemaestreb.github.io/docs.pia/_asset/03_transporte/07-top-destinatarios.png" alt="Top de Destinatarios Con Mayor Incumplimiento" loading="lazy"/> 

---

## Otras Funcionalidades

¡Excelente! Si has llegado hasta aquí, ya estás listo para interactuar con el tablero de manera segura y eficiente. A continuación, te presentamos unas últimas funciones clave para aprovechar al máximo la herramienta.

---

### • Menú de Filtros

Para abrir el menú de filtros, simplemente haz clic en el botón indicado en la siguiente imagen:

<img src="https://josemaestreb.github.io/docs.pia/_asset/03_transporte/08-menu-filtros.png" alt="Abrir Filtros PIA Transporte" loading="lazy"/> 

Este menú te permite personalizar la visualización de los datos. Como ejemplo, te mostramos cómo:

**1-** Aplicar un filtro de fecha a partir del **31/08/2024**.  
**2-** Cambiar la unidad de medida a **"Pedidos"**.  
**3-** Aplicar un filtro por tipo de consolidado seleccionando los pedidos de **"Ciclo"**.

<img src="https://josemaestreb.github.io/docs.pia/_asset/03_transporte/09-aplicacion-de-filtros.gif" alt="Filtros PIA Transporte" loading="lazy"/> 

<p class="tip">En este menú de filtros, puedes personalizar la visualización de las cantidades eligiendo la unidad de medida que prefieras: <strong>Cajas, Pedidos o Unidades</strong>. Simplemente ajusta la opción en <strong>"Unidades / Pedidos / Cajas"</strong>.</p>

---

### • Consulta Detalles de los Incumplimientos

Accede fácilmente a información detallada sobre los pedidos que no cumplieron con los tiempos de entrega (On Time) o que no fueron entregados completos y sin defectos (In Full), junto con las causas asociadas. Sigue estos pasos para visualizar los datos directamente en el tablero:  

<img src="https://josemaestreb.github.io/docs.pia/_asset/03_transporte/10-funciones-adicionales.gif" alt="Detalle y Visual de Causales PIA Transporte" loading="lazy"/>  

---

## 🎉 ¡Módulo Completado!

¡Felicidades! Ahora comprendes la estructura y funcionalidad básica del tablero de Transporte. En los próximos módulos, profundizaremos en temas avanzados como fórmulas, fuentes de datos y medidas DAX para maximizar tu análisis.
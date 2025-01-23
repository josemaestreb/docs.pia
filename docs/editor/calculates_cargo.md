# 🧮 Cálculos y Fórmulas Aplicadas

Este documento detalla los cálculos y procesos utilizados en el tablero de transporte. Proporciona el análisis de datos clave, alineado con las reglas de negocio definidas por **PHAREX** y sus clientes.

Gracias a estos cálculos, es posible determinar horarios de corte, cantidad de días, desde la facturación hasta la entrega, y métricas relacionadas al cumplimiento.

### 💡 Contexto del Proceso
Los cálculos son ejecutados en Power Query, la herramienta de transformación de datos en Power BI. Se accede a estos cálculos a través del editor avanzado de la consulta llamada `"API_BD_Cargo"`.

La lógica de evaluación permite determinar el cumplimiento de las entregas mediante un análisis estructurado, lógico y ajustado a las reglas de negocio.

### 🔄 Pasos Generales del Proceso

**1. Identificación del Corte de Alistamiento:** Se determina el corte correspondiente al pedido según la columna `HORA_FACTURA`.  
**2. Cálculo de Días de Entrega:** `(DIAS_ENTREGA)`.  
**3. Evaluación de las Condiciones ON TIME:** Aplicando reglas predefinidas en orden lógico hasta identificar la que se cumple primero.  
**3. Evaluación del Indicador IN FULL:** Se considera "No Cumple" si existe una causal identificada que afecte el indicador, como "Mercancía Averiada".  
**4. Resultado Final:** Clasificación del On Time y del In Full como "Cumple" o "No Cumple", según corresponda.  

<p class="tip">Estos pasos son ejecutados de manera automática por Power BI, lo que garantiza un análisis lógico y claro. A continuación, se profundiza en cada una de estas etapas para una mejor comprensión. 🚀</p>

---

## 📊 1. Manejo de los Cortes

El cálculo de los cortes depende de la columna `HORA_FACTURA` y el código de división `CODDIV`. A continuación, se detallan los rangos generales y los casos específicos:

---

### 1.1. **Cortes Generales**

Los cortes se definen en los siguientes rangos:

- **Primer Corte:** `7:00 a.m. - 10:00 a.m.`  
- **Segundo Corte:** `10:00 a.m. - 2:00 p.m.`  
- **Fuera de Corte:** A partir de las `2:00 p.m.`  

<p class="tip">En el tablero de transporte, el primer corte se considera a partir de las <strong>4:00 a.m.</strong> Esto se hace con el objetivo de incluir todos los pedidos que ingresan a <strong>IP6</strong> antes de las 7:00 a.m., evitando que queden excluidos en el análisis.</p>

---

### 1.2. **Reglas por Código de División (`CODDIV`)**

Cada código de división tiene un conjunto de reglas específicas:

**Nota:** Para consultar los nombres de las divisiones relaciones a continuación, puede dirigirse a la sección de [Preguntas Frecuentes](faq.md)

#### Caso A: `CODDIV = 0075`
- Primer Corte: `7:00 a.m. - 10:00 a.m.`  
- Segundo Corte: `10:00 a.m. - 12:00 p.m.`  
- Tercer Corte: `12:00 p.m. - 2:00 p.m.`  
- Fuera de Corte: A partir de las `2:00 p.m.`  

---

#### Caso B: `CODDIV = 0021`
- Primer Corte: `7:00 a.m. - 10:00 a.m.`  
- Segundo Corte: `10:00 a.m. - 3:00 p.m.`  
- Fuera de Corte: A partir de las `3:00 p.m.`  

---

#### Caso C: `CODDIV = 0041`
- Primer Corte: `7:00 a.m. - 10:00 a.m.`  
- Segundo Corte: `10:00 a.m. - 1:00 p.m.`  
- Fuera de Corte: A partir de la `1:00 p.m.`  

---

## 📆 2. Cálculo de Días de Entrega (`DIAS_ENTREGA`)

Los días de entrega se calculan como el número de días hábiles transcurridos entre la fecha de factura (`FECHA_FACTURA`) y la fecha de entrega (`FECHA_ENTREGA`). 

### 2.1. **Consideraciones Importantes**

- **Si no existe fecha de entrega:** Se utiliza la fecha actual como referencia.  
- **Se restará un día adicional** si el pedido se considera "Fuera de Corte".  
- **Se excluyen los fines de semana** y se ajustan los días restando la cantidad de festivos que haya entre la fecha de factura (`FECHA_FACTURA`) y la fecha de entrega (`FECHA_ENTREGA`).  
    - **Nota:** La fecha de factura es considerada festiva, si aparece en la tabla de `DIAS_FES` de la base de datos de IP6.

---

## 🚀 3. Cálculo de Cumplimiento: `ON TIME`

Este campo evalúa si el pedido cumple con las reglas establecidas para los tiempos de entrega. Se analizan múltiples condiciones para validar su cumplimiento y determinar si el proceso es correcto y se ajusta a los criterios definidos. Las condiciones consideradas son:

---

### 3.1. **Regla 1: Entrega a Tiempo Según Cita / Promesa**

Una entrega se considera dentro de los tiempos establecidos si cumple con las dos primeras condiciones que se detallan a continuación, o con la última:
 
- Existen fechas definidas para `FECHA_ENTREGA` y `FECHA_CITA`.  
- `FECHA_ENTREGA ≤ FECHA_CITA`.  
- `DIAS_ENTREGA ≤ PROMESA`.  

**Código relacionado:**
```dax
each if ([FECHA_ENTREGA] <= [FECHA_CITA] and [FECHA_ENTREGA] <> null and [FECHA_CITA] <> null) or 
   ([PROMESA] <> null and [DIAS_ENTREGA] <= [PROMESA])
```

---

### 3.2. **Regla 2: Entrega Inmediata o Consolidado en CEDI**

Una entrega se considera a tiempo si se cumple al menos una de las siguientes condiciones:  

- `DIAS_ENTREGA <= 1`.  
- El tipo de consolidado es `Cedi`.  
- El `CODDIV` es 0021, el tipo de consolidado es `Comercial` y `DIAS_ENTREGA <= 2`.  
- El tipo de consolidado es `Muestra Médica` o `Ciclo` y `DIAS_ENTREGA <= 5`.  
- El `CODDIV` es `0055` o `0043`, el tipo de consolidado es `Especial` y `DIAS_ENTREGA <= 5`.  

**Código relacionado:**
```dax
if [DIAS_ENTREGA] <= 1 or 
           [TIPO DE CONSOLIDADO] = "Cedi" or 
           ([CODDIV] = "0021" and [TIPO DE CONSOLIDADO] = "Comercial" and [DIAS_ENTREGA] <= 2) or
           (List.Contains({"Ciclo", "Muestra Medica"}, [TIPO DE CONSOLIDADO]) and [DIAS_ENTREGA] <= 5) or
           (List.Contains({"0055", "0043"}, [CODDIV]) and [TIPO DE CONSOLIDADO] = "Especial" and [DIAS_ENTREGA] <= 5) then 
            "Cumple"
```

---

### 3.3. **Regla 3: Condiciones Especiales de Consolidado**

Se considera que una entrega cumple los tiempos si se presenta alguna de las siguientes situaciones:

1. El tipo de consolidado es uno de los siguientes:  
   - Especial  
   - LoveISDIN  
   - Tester  
   Y el número de días de transporte es **menor o igual a 3**.

2. El código de división es `0091`, el tipo de consolidado es `Retail`, y se aplican las siguientes reglas según el día de entrega (`FECHA_ENTREGA`) y si es un día festivo (`FECHA_FACT_FEST`):  

   - Si **no es un día festivo**, la fecha de entrega debe ser un miércoles.  
   - Si **es un día festivo**, la fecha de entrega debe ser un jueves.

3. La fecha de entrega debe ser anterior o igual a la fecha de cita (`FECHA_CITA`) siempre y cuando ambas fechas estén disponibles.

---

**Código relacionado:**
```dax
if (List.Contains({"Especial", "LoveISDIN", "Tester"}, [TIPO_DE_CONSOLIDADO]) and [DIAS_ENTREGA] <= 3) or 
               ([CODDIV] = "0091" and [TIPO_DE_CONSOLIDADO] = "Retail" and [FECHA_FACT_FEST] = "N" and 
                not (Record.FieldOrDefault(_, "FECHA_ENTREGA") is null) and 
                Date.DayOfWeek([FECHA_ENTREGA], Day.Monday) = 3) or 
               ([CODDIV] = "0091" and [TIPO_DE_CONSOLIDADO] = "Retail" and [FECHA_FACT_FEST] = "S" and 
                not (Record.FieldOrDefault(_, "FECHA_ENTREGA") is null) and 
                Date.DayOfWeek([FECHA_ENTREGA], Day.Monday) = 4) or
               ([FECHA_ENTREGA] <= [FECHA_CITA] and 
                not (Record.FieldOrDefault(_, "FECHA_ENTREGA") is null) and 
                not (Record.FieldOrDefault(_, "FECHA_CITA") is null)) then
                "Cumple"
```

---

### 3.4. **Regla 4: En Caso de Incumplimiento**

En situaciones donde **ninguna de las reglas anteriores** se cumple, el ON TIME se considera como **No Cumple** en el análisis de la columna `ON TIME`.

---

## 📊 4. Cálculo de Cumplimiento: `IN FULL`

Se considera "No Cumple" si existe una causal identificada que afecte el indicador, como "Mercancía Averiada", en el campo `IN FULL`.

---


## 📌 Otras Consideraciones  

El tipo de consolidado o clasificación del pedido se identifica a partir de reglas específicas basadas en el código de división (`CODDIV`) y patrones en el código del pedido (`CODPED`). A continuación se resumen las condiciones:

---

### 📝 **Reglas para identificar el tipo de consolidado:**  

1. **Muestra Médica:**  
   - Si `CODDIV = '0005'` **Y** las dos primeras letras de `CODPED` son `'MM'`.  
   - Si `CODDIV = '0068'` **Y** las dos primeras letras de `CODPED` son `'OP'`.  

2. **Ciclo:**  
   - Si `CODDIV = '0075'` **Y** las tres primeras letras de `CODPED` son `'SAL'`.  
   - Si `CODDIV = '0015'` **Y** las dos primeras letras de `CODPED` son `'SA'`.  
   - Si `CODDIV = '0083'` **Y** las tres primeras letras de `CODPED` son `'REQ'`.  
   - Si `CODDIV = '0041'` **Y** las tres primeras letras de `CODPED` son `'OUT'`.  

3. **Especial:**  
   - Si `CODDIV = '0055'` **Y** las dos primeras letras de `CODPED` son `'SA'`.  
   - Si `CODDIV = '0041'` **Y** las tres primeras letras de `CODPED` son `'SVI'`.  

4. **Tester:**  
   - Si `CODDIV = '0035'` **Y** el código de pedido contiene la letra `'R'`.  

5. **Retail vs. B2B:**  
   - Si `CODDIV = '0091'`:  
     - Si `CODPED` contiene `'PT'` o `'GA'`, se clasifica como **Retail**.  
     - De lo contrario, se clasifica como **B2B**.  

6. **Por defecto:**  
   - Si ninguna de las condiciones anteriores se cumple, se consulta una tabla externa para determinar el tipo de consolidado basado en el tipo de pedido (`TIPOPED`).  

**Consulta SQL relacionada:**
```sql
CASE 
      WHEN A.CODDIV = '0005' AND SUBSTR(A.CODPED,0,2) = 'MM' THEN 'Muestra Medica'
		WHEN A.CODDIV = '0075' AND SUBSTR(A.CODPED,0,3) = 'SAL' THEN 'Ciclo'
		WHEN A.CODDIV = '0015' AND SUBSTR(A.CODPED,0,2) = 'SA' THEN 'Ciclo'
		WHEN A.CODDIV = '0083' AND SUBSTR(A.CODPED,0,3) = 'REQ' THEN 'Ciclo'
		WHEN A.CODDIV = '0055' AND SUBSTR(A.CODPED,0,2) = 'SA' THEN 'Especial'
		WHEN A.CODDIV = '0035' AND A.CODPED LIKE '%R%' THEN 'Tester'
		WHEN A.CODDIV = '0068' AND SUBSTR(A.CODPED,0,2) = 'OP' THEN 'Muestra Medica'
		WHEN A.CODDIV = '0041' AND SUBSTR(A.CODPED,0,3) = 'OUT' THEN 'Ciclo'
		WHEN A.CODDIV = '0041' AND SUBSTR(A.CODPED,0,3) = 'SVI' THEN 'Especial'
		WHEN A.CODDIV = '0091' AND (A.CODPED LIKE '%PT' OR A.CODPED LIKE '%GA') THEN 'Retail'
		WHEN A.CODDIV = '0091' AND NOT (A.CODPED LIKE '%PT' OR A.CODPED LIKE '%GA') THEN 'B2B'
   ELSE (SELECT D.TIPO_CONSOLIDADO FROM XIPTIPOCONSOLDINTEG D WHERE D.TIPOPED = A.TIPOPED)
END AS TIPO_DE_CONSOLIDADO,
```
---

## 🎉 ¡Módulo Completado!

¡Excelente! Ahora tienes una visión completa de los cálculos, su lógica y reglas. Estás preparado para interpretar los datos, analizar métricas clave y aprovechar toda la información que ofrece el tablero de transporte. 🚀
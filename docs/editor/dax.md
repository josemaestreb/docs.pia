# 🧮 Medidas DAX

El tablero de recepciones utiliza un conjunto de fórmulas DAX (Data Analysis Expressions), diseñadas para realizar cálculos avanzados y precisos. Estas medidas permiten un análisis profundo de los datos sin realizar modificaciones en el origen de datos ni afectar el modelo semántico subyacente.

A continuación, se presenta una descripción detallada de las medidas DAX utilizadas en el tablero, con su respectiva funcionalidad y fórmula correspondiente:

## 1️⃣ Total Unidades
Calcula la cantidad total de unidades recibidas en cada operación de recepción.
```dax
Total Unidades = SUM('JSON Recepcion'[CANTIDAD])
```

## 2️⃣ Total Cajas
Suma la cantidad total de cajas recibidas en cada operación de recepción.
```dax
Total Cajas = SUM('JSON Recepcion'[CANTIDAD_CAJAS])
```

## 3️⃣ Total Líneas
Cuenta el total de líneas en los datos de cada operación de recepción.
```dax
Total Lineas = COUNTROWS('JSON Recepcion')
```

## 4️⃣ Promedio de Días de Recepción
Calcula el promedio de días tomados para la recepción. Si no hay datos, devuelve 0.
```dax
Promedio Dias Rec = IF(ISBLANK(AVERAGE('BD Recepción'[DIAS REC])), 0, AVERAGE('BD Recepción'[DIAS REC]))
```

## 5️⃣ Promedio de Días de Calidad
Calcula el promedio de días invertidos en las inspecciones de calidad. Si no hay datos, devuelve 0.
```dax
Promedio Dias Cal = IF(ISBLANK(AVERAGE('BD Recepción'[DIAS CAL])), 0, AVERAGE('BD Recepción'[DIAS CAL]))
```

## 6️⃣ Promedio de Días de Liberación
Promedio de días desde la recepción hasta la liberación de productos.
```dax
Promedio DiasRec-Cal = AVERAGE('BD Recepción'[DIAS REC-CAL])
```

## 7️⃣ Pedidos que Incumplen Tiempos de Recepción
Calcula el total de pedidos que no cumplen con el tiempo óptimo de recepción (menos de 3 días hábiles).
```dax
Total Pedidos_NoCumplen = 
CALCULATE(
        DISTINCTCOUNT('BD Recepción'[DOC_CLIENTE]),
        'BD Recepción'[CUMPLIMIENTO REC] = "No Cumple")
```

## 8️⃣ Porcentaje de Cumplimiento de Recepción
Determina el porcentaje de recepciones que cumplen con los tiempos establecidos para recibir la mercancía.
```dax
Porcentaje_Cumplimiento_REC = 
DIVIDE(
    CALCULATE(
        DISTINCTCOUNT('BD Recepción'[DOC_CLIENTE]),
        'BD Recepción'[CUMPLIMIENTO REC] = "Cumple"
    ),
    DISTINCTCOUNT('BD Recepción'[DOC_CLIENTE]))
```

## 9️⃣ Porcentaje de Cumplimiento de Calidad
Evalúa el porcentaje de recepciones que cumplen con los tiempos óptimos de inspección y envío del parte de calidad (menos de 2 días hábiles).
```dax
Porcentaje_Cumplimiento_Calidad = 
DIVIDE(
    CALCULATE(
        DISTINCTCOUNT('BD Recepción'[DOC_CLIENTE]),
        'BD Recepción'[CUMPLIMIENTO CALIDAD] = "Cumple"
    ),
    DISTINCTCOUNT('BD Recepción'[DOC_CLIENTE]))
```

## 🔟 Meta Días de Recepción
Establece la meta de días para la recepción de mercancía.
```dax
Meta Cumplimiento DiasRec = 3
```

## 1️⃣1️⃣ Meta Porcentaje de Cumplimiento de Recepción
Devuelve la meta de porcentaje de cumplimiento de recepción, ajustada según el cliente seleccionado.
```dax
Meta_%Cumplimiento_Rec = 
VAR SelectedCliente = SELECTEDVALUE('DimCliente'[CLIENTE])
VAR MetaCump = SELECTEDVALUE(DimCliente[Meta Cumplimiento Recepcion])
RETURN
IF(
    ISFILTERED('DimCliente'[CLIENTE]) && NOT(ISBLANK(SelectedCliente)),
    MetaCump,
    0.95
)
```

## 1️⃣2️⃣ Faltante Meta de Cumplimiento de Recepción
Calcula la diferencia entre la meta y el porcentaje real de cumplimiento en recepción.
```dax
Faltante para Cump REC = [Meta_%Cumplimiento_Rec] - [Porcentaje_Cumplimiento_REC]
```

## 1️⃣3️⃣ Faltante Meta de Cumplimiento de Calidad
Calcula la diferencia entre la meta y el porcentaje real de cumplimiento en calidad.
```dax
Faltante para Cump Calidad = [Meta_%Cumplimiento_Rec] - [Porcentaje_Cumplimiento_Calidad]
```

## 1️⃣4️⃣ Eficiencia Días de Recepción
Mide la eficiencia comparando el promedio de días reales con la meta definida para la recepción.
```dax
Eficiencia Dias Rec = [Promedio DiasRec-Cal] - [Meta Cumplimiento DiasRec]
```

## 1️⃣5️⃣ Logo de Clientes
Muestra el logo correspondiente al cliente en el tablero. Si no se selecciona ningún cliente, usa el logo de PHAREX por defecto.
```dax
Logo_Rec = 
VAR SelectedCliente = SELECTEDVALUE('DimCliente'[CLIENTE])
VAR Logo = SELECTEDVALUE(DimCliente[Logo])
RETURN
IF(
    ISFILTERED('DimCliente'[CLIENTE]) && NOT(ISBLANK(SelectedCliente)),
    Logo,
    "https://josemaestreb.github.io/01_Pharex_Workspace/Logos_Clientes/PHAREX.png"
)
```

## 1️⃣6️⃣ Ajuste de Unidades de Medición (Switch)
Permite ajustar el análisis entre unidades, cajas y líneas de forma dinámica según el contexto seleccionado.
```dax
Switch Tipo_REC = 
VAR SelectedContext = SELECTEDVALUE(DimTipoMedida[Tipo])
VAR Resultado = 
    SWITCH(
        SelectedContext,
        "Unidades", [Total Unidades],
        "Cajas", [Total Cajas],
        "Lineas", [Total Lineas],
        [Total Unidades]
    )
RETURN
Resultado
```

---

## 🎉 ¡Módulo Completado!

¡Excelente trabajo! Ahora comprendes cómo las medidas DAX potencian el análisis en el tablero de recepciones. Estás listo para explorar, interpretar y aprovechar al máximo las métricas clave en tu análisis. 🚀
# 🧮 Medidas DAX

El tablero de transporte utiliza un conjunto de fórmulas DAX (Data Analysis Expressions), diseñadas para realizar cálculos avanzados y precisos. Estas medidas permiten un análisis profundo de los datos sin realizar modificaciones en el origen de datos ni afectar el modelo semántico subyacente.

A continuación, se presenta una descripción detallada de las medidas DAX utilizadas en el tablero, con su respectiva funcionalidad y fórmula correspondiente:

## 1️⃣ Total Unidades
Calcula la cantidad total de unidades servidas.
```dax
Total_Unidades_Cargo = SUM('API_BD_Cargo'[TOTAL_UNIDADES_SERVIDAS])
```

## 2️⃣ Total Cajas
Suma el total de cajas alistadas en todos los pedidos registrados.
```dax
Total_Cajas_Cargo = SUM('API_BD_Cargo'[TOTAL_CAJAS])
```

## 3️⃣ Total Pedidos
Cuenta la cantidad total de pedidos únicos disponibles en el modelo semántico.
```dax
Total_Pedidos_Cargo = DISTINCTCOUNT('API_BD_Cargo'[CODIGO_PEDIDO])
```

## 4️⃣ Total Pedidos Cumple OTIF
Filtra y cuenta el total de pedidos que **cumplen** con la métrica de OTIF.
```dax
Total_Pedidos_CumpleOTIF_Cargo = 
CALCULATE(DISTINCTCOUNT('API_BD_Cargo'[CODIGO_PEDIDO]),
'API_BD_Cargo'[OTIF] = "Cumple")
```

## 5️⃣ Total Pedidos No Cumple OTIF
Filtra y cuenta el total de pedidos que **no cumplen** con la métrica de OTIF.
```dax
Total_Pedidos_NoCumpleOTIF_Cargo = 
CALCULATE(DISTINCTCOUNT('API_BD_Cargo'[CODIGO_PEDIDO]),
'API_BD_Cargo'[OTIF] = "No Cumple")
```

## 6️⃣ Total Pedidos No Cumplen On Time
Cuenta los pedidos que **no cumplen** con la métrica de On Time.
```dax
Total_Pedidos_NoCumpleOnTime_Cargo = 
CALCULATE(DISTINCTCOUNT('API_BD_Cargo'[CODIGO_PEDIDO]),
'API_BD_Cargo'[ON TIME] = "No Cumple")
```

## 7️⃣ Total Pedidos No Cumplen In Full
Cuenta los pedidos que **no cumplen** con la métrica del In Full.
```dax
Total_Pedidos_NoCumpleInFull_Cargo = 
CALCULATE(DISTINCTCOUNT('API_BD_Cargo'[CODIGO_PEDIDO]),
'API_BD_Cargo'[IN FULL] = "No Cumple")
```

## 8️⃣ Porcentaje de Cumplimiento In Full
Calcula el porcentaje de pedidos entregados de manera completa y sin defectos, cumpliendo con las condiciones establecidas.
```dax
% Cumplimiento InFull = 
DIVIDE(
    COUNTROWS(FILTER('API_BD_Cargo', 'API_BD_Cargo'[IN FULL] = "Cumple")),
    COUNTROWS('API_BD_Cargo')
)
```

<p class="tip">La misma lógica se aplica a las medidas utilizadas para calcular los porcentajes de cumplimiento de ON TIME y OTIF, ajustándose a los campos correspondientes de cada indicador.</p>

## 9️⃣ Porcentaje de Cumplimiento In Full (Cliente)
Porcentaje de cumplimiento 'In Full' de pedidos no afectados por responsabilidad del cliente, considerando solo aquellos que cumplieron con todas las condiciones.
```dax
% Cumplim. InFull CLIENTE = 
1 - DIVIDE(
    COUNTROWS(FILTER('API_BD_Cargo', 'API_BD_Cargo'[IN FULL] = "No Cumple" && 'API_BD_Cargo'[RESPONSABLE] = "CLIENTE")),
    COUNTROWS('API_BD_Cargo')
)
```

<p class="tip">La misma lógica se aplica para otras categorías de responsables como <strong>DESTINATARIO, FACTOR EXTERNO y PHAREX</strong>, adaptando la condición del campo [RESPONSABLE] en la medida DAX. Asimismo, existen medidas similares aplicables para evaluar los indicadores ON TIME y OTIF, adaptadas a sus respectivos campos.</p>

## 🔟 Meta Cumplimiento On Time / In Full / OTIF
Establece un valor predeterminado del 87% como meta de cumplimiento para los KPI clave.
```dax
Meta Cump OnTime/InFull/OTIF = 0.87
```

## 1️⃣1️⃣ Logo de Clientes
Muestra el logo correspondiente al cliente en el tablero. Si no se selecciona ningún cliente, usa el logo de PHAREX por defecto.
```dax
Logo_Cargo = 
VAR SelectedCliente = SELECTEDVALUE('DimCliente'[CLIENTE])
VAR Logo = SELECTEDVALUE(DimCliente[Logo])
RETURN
IF(
    ISFILTERED('DimCliente'[CLIENTE]) && NOT(ISBLANK(SelectedCliente)),
    Logo,
    "https://josemaestreb.github.io/01_Pharex_Workspace/Logos_Clientes/PHAREXCargo.png"
)
```

## 1️⃣2️⃣ Ajuste de Unidades de Medición (Switch)
Permite cambiar entre unidades, cajas y pedidos dinámicamente según el contexto seleccionado en el tablero. Si no se selecciona ninguna opción, por predeterminado escoge el total de unidades.
```dax
Switch Tipo_Cargo = 
VAR SelectedContext = SELECTEDVALUE(DimTipoMedida[Tipo])
RETURN
    SWITCH(
        SelectedContext,
        "Unidades", [Total_Unidades_Cargo],
        "Cajas", [Total_Cajas_Cargo],
        "Pedidos", [Total_Pedidos_Cargo],
        [Total_Unidades_Cargo]
    )
```

## 1️⃣3️⃣ Faltante Cumplimiento Cargo
Calcula la diferencia entre la meta de cumplimiento y el porcentaje general de cumplimiento actual.
```dax
Faltante Cump Cargo = [Meta Cump OnTime/InFull/OTIF] - [Switch % Cumplim. General]
```

## 1️⃣4️⃣ Ajuste de Cumplimiento General (Switch)
Cambia dinámicamente entre los indicadores de cumplimiento On Time, In Full y OTIF según el contexto seleccionado.
```dax
Switch % Cumplim. General = 
VAR SelectedContext = SELECTEDVALUE(DimTipoCumplimiento[Tipo])
RETURN
    SWITCH(
        SelectedContext,
        "On Time", [% Cumplimiento OnTime],
        "In Full", [% Cumplimiento InFull],
        "OTIF", [% Cumplimiento OTIF],
        [% Cumplimiento OnTime]
    )
```


## 1️⃣5️⃣ Ajuste de Cumplimiento de Cliente (Switch)
Porcentaje promedio de pedidos no afectados por responsabilidad del cliente en On Time, In Full y OTIF, calculado según el contexto seleccionado.
```dax
Switch % Cumplim. CLIENTE = 
VAR SelectedContext = SELECTEDVALUE(DimTipoCumplimiento[Tipo])
RETURN
    SWITCH(
        SelectedContext,
        "On Time", [% Cumplim. OnTime CLIENTE],
        "In Full", [% Cumplim. InFull CLIENTE],
        "OTIF", [% Cumplim. OTIF CLIENTE],
        [% Cumplim. OnTime CLIENTE]
    )
```

<p class="tip">La misma lógica se aplica para otras categorías de responsables como <strong>DESTINATARIO, FACTOR EXTERNO y PHAREX</strong>, adaptando la condición del campo [RESPONSABLE] en la medida DAX.</p>

## 1️⃣6️⃣ Ajuste de Cumplimiento de Volumen (Switch)
Define metas de volumen específicas (unidades, cajas o pedidos) según el contexto seleccionado en el tablero.
```dax
Switch Meta Cump Cargo = 
VAR SelectedContext = SELECTEDVALUE(DimTipoMedida[Tipo])
RETURN
    SWITCH(
        SelectedContext,
        "Unidades", 180000,
        "Cajas", 1900,
        "Pedidos", 800,
        180000
    )
```

## 1️⃣7️⃣ Ajuste de Meta Cumplimiento General (Switch)
Ajusta dinámicamente las metas de cumplimiento On Time, In Full y OTIF, según el contexto seleccionado en el tablero.
```dax
Switch Meta Cumplim. General = 
VAR SelectedContext = SELECTEDVALUE(DimTipoCumplimiento[Tipo])
RETURN
    SWITCH(
        SelectedContext,
        "On Time", 0.95,
        "In Full", 0.98,
        "OTIF", 0.93,
        0.95
    )
```

## 1️⃣8️⃣ Ajuste de Cantidad Pedidos por Incumplimiento (Switch)
Cambia entre el total de pedidos que no cumplen con On Time, In Full o OTIF según el indicador seleccionado.
```dax
Switch Pedido_NoCumple = 
VAR SelectedContext = SELECTEDVALUE(DimTipoCumplimiento[Tipo])
RETURN
    SWITCH(
        SelectedContext,
        "On Time", [Total_Pedidos_NoCumpleOnTime_Cargo],
        "In Full", [Total_Pedidos_NoCumpleInFull_Cargo],
        "OTIF", [Total_Pedidos_NoCumpleOTIF_Cargo],
        [Total_Pedidos_NoCumpleOnTime_Cargo]
    )
```

---

## 🎉 ¡Módulo Completado!

¡Excelente trabajo! Ahora comprendes cómo las medidas DAX potencian el análisis en el tablero de transporte. Estás listo para explorar, interpretar y aprovechar al máximo las métricas clave en tu análisis. 🚀
# 🧮 Medidas DAX

El tablero de alistamiento utiliza un conjunto de fórmulas DAX (Data Analysis Expressions), diseñadas para realizar cálculos avanzados y precisos. Estas medidas permiten un análisis profundo de los datos sin realizar modificaciones en el origen de datos ni afectar el modelo semántico subyacente.

A continuación, se presenta una descripción detallada de las medidas DAX utilizadas en el tablero, con su respectiva funcionalidad y fórmula correspondiente:

## 1️⃣ Total Unidades
Calcula la cantidad total de unidades pedidas por el cliente.
```dax
Total Unidades Ped = SUM('API BD Alistamiento'[TOTAL_UNIDADES_PEDIDAS])
```

## 2️⃣ Total Cajas
Suma el total de cajas alistadas en todos los pedidos registrados.
```dax
Total Cajas Alis = SUM('API BD Alistamiento'[TOTAL_CAJAS])
```

## 3️⃣ Total Pedidos
Cuenta la cantidad total de pedidos únicos disponibles en el modelo semántico.
```dax
Total Pedidos = DISTINCTCOUNT('API BD Alistamiento'[CODIGO_PEDIDO])
```

## 4️⃣ Total Pedidos de Ciclo
Filtra y cuenta el total de pedidos cuyo tipo de consolidado es "CICLO".
```dax
Total Pedidos Ciclo = 
IF(
    ISBLANK(
        CALCULATE(
            DISTINCTCOUNT('API BD Alistamiento'[CODIGO_PEDIDO]),
            'API BD Alistamiento'[TIPO DE CONSOLIDADO] = "CICLO"
        )
    ),
    0,
    CALCULATE(
        DISTINCTCOUNT('API BD Alistamiento'[CODIGO_PEDIDO]),
        'API BD Alistamiento'[TIPO DE CONSOLIDADO] = "CICLO"
    )
)
```

## 5️⃣ Total Pedidos Comerciales
Filtra y cuenta el total de pedidos cuyo tipo de consolidado es "COMERCIAL".
```dax
Total Pedidos Comercial = 
IF(
    ISBLANK(
        CALCULATE(
            DISTINCTCOUNT('API BD Alistamiento'[CODIGO_PEDIDO]),
            'API BD Alistamiento'[TIPO DE CONSOLIDADO] = "COMERCIAL"
        )
    ),
    0,
    CALCULATE(
        DISTINCTCOUNT('API BD Alistamiento'[CODIGO_PEDIDO]),
        'API BD Alistamiento'[TIPO DE CONSOLIDADO] = "COMERCIAL"
    )
)
```

## 6️⃣ Total Pedidos Cumplen OTS
Cuenta los pedidos que **cumplen** con la métrica de On Time Shipe (OTS).
```dax
Total Ped Cumplen = CALCULATE(
        DISTINCTCOUNT('API BD Alistamiento'[CODIGO_PEDIDO]),
        'API BD Alistamiento'[ON TIME SHIPE] = "Cumple")
```

## 7️⃣ Total Pedidos No Cumplen OTS
Cuenta los pedidos que **no cumplen** con la métrica del On Time Shipe.
```dax
Total Ped NoCumplen = CALCULATE(
        DISTINCTCOUNT('API BD Alistamiento'[CODIGO_PEDIDO]),
        'API BD Alistamiento'[ON TIME SHIPE] = "No Cumple")
```

## 8️⃣ Promedio de Capacidad
Calcula el promedio de cumplimiento de la capacidad de alistamiento.
```dax
Promedio Capacidad = AVERAGE('Capacidad Alis'[CUMPLIMIENTO CAPACIDAD])
```

## 9️⃣ Porcentaje de Cumplimiento de OTS
Calcula el porcentaje de pedidos que cumplen con los tiempos establecidos para el alistamiento.
```dax
OnTimeShip % Cump = 
VAR SelectedContext = SELECTEDVALUE('API BD Alistamiento'[ON TIME SHIPE])
RETURN
    SWITCH(
        TRUE(),
        SelectedContext = "Cumple",
        DIVIDE(
            CALCULATE(
                DISTINCTCOUNT('API BD Alistamiento'[CODIGO_PEDIDO]),
                'API BD Alistamiento'[ON TIME SHIPE] = "Cumple"
            ),
            DISTINCTCOUNT('API BD Alistamiento'[CODIGO_PEDIDO])
        ),
        SelectedContext = "No Cumple",
        DIVIDE(
            CALCULATE(
                DISTINCTCOUNT('API BD Alistamiento'[CODIGO_PEDIDO]),
                'API BD Alistamiento'[ON TIME SHIPE] = "No Cumple"
            ),
            DISTINCTCOUNT('API BD Alistamiento'[CODIGO_PEDIDO])
        ),
        DIVIDE(
            CALCULATE(
                DISTINCTCOUNT('API BD Alistamiento'[CODIGO_PEDIDO]),
                'API BD Alistamiento'[ON TIME SHIPE] = "Cumple"
            ),
            DISTINCTCOUNT('API BD Alistamiento'[CODIGO_PEDIDO])
        )
    )
```

## 🔟 Meta Cumplimiento OTS
Recupera la meta de cumplimiento de OTS configurada para el cliente seleccionado. Si no hay un cliente específico, usa el valor por defecto del 87%.
```dax
Meta_%Cumplimiento_OTS = 
VAR SelectedCliente = SELECTEDVALUE('DimCliente'[CLIENTE])
VAR MetaCump = SELECTEDVALUE(DimCliente[Meta Cumplimiento OnTimeShipe])
RETURN
IF(
    ISFILTERED('DimCliente'[CLIENTE]) && NOT(ISBLANK(SelectedCliente)),
    MetaCump,
    0.87
)
```

## 1️⃣1️⃣ Logo de Clientes
Muestra el logo correspondiente al cliente en el tablero. Si no se selecciona ningún cliente, usa el logo de PHAREX por defecto.
```dax
Logo_Alis = 
VAR SelectedCliente = SELECTEDVALUE('DimCliente'[CLIENTE])
VAR Logo = SELECTEDVALUE(DimCliente[Logo])
RETURN
IF(
    ISFILTERED('DimCliente'[CLIENTE]) && NOT(ISBLANK(SelectedCliente)),
    Logo,
    "https://josemaestreb.github.io/01_Pharex_Workspace/Logos_Clientes/PHAREX.png"
)
```

## 1️⃣2️⃣ Ajuste de Unidades de Medición (Switch)
Permite cambiar entre unidades, cajas y pedidos dinámicamente según el contexto seleccionado en el tablero.
```dax
Switch Tipo_Alis = 
VAR SelectedContext = SELECTEDVALUE(DimTipoMedida[Tipo])
RETURN
    SWITCH(
        SelectedContext,
        "Unidades", [Total Unidades Ped],
        "Cajas", [Total Cajas Alis],
        "Pedidos", [Total Pedidos],
        [Total Unidades Ped]
    )
```

---

## 🎉 ¡Módulo Completado!

¡Excelente trabajo! Ahora comprendes cómo las medidas DAX potencian el análisis en el tablero de alistamiento. Estás listo para explorar, interpretar y aprovechar al máximo las métricas clave en tu análisis. 🚀
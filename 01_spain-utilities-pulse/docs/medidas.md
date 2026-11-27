# Medidas y objetos DAX · Spain Utilities Pulse

Referencia completa del modelo semántico, generada a partir del export de expresiones DAX del modelo. Contiene **114 medidas**, **6 tablas calculadas** y **2 columnas calculadas**, sin errores.

## Índice

1. [Relaciones](#1-relaciones)
2. [Tablas y columnas calculadas](#2-tablas-y-columnas-calculadas)
3. [Sector activo, colores e iconos](#3-sector-activo-colores-e-iconos) · 15
4. [Electricidad](#4-electricidad) · 21
5. [Gas natural y almacenamiento](#5-gas-natural-y-almacenamiento) · 26
6. [Agua](#6-agua) · 22
7. [Gráficos y frescura](#7-gráficos-y-frescura) · 10
8. [Banner](#8-banner) · 10
9. [Tarjetas laterales](#9-tarjetas-laterales) · 2
10. [Indicadores con volteo](#10-indicadores-con-volteo) · 2
11. [HTML](#11-html) · 6

## 1. Relaciones

| Desde (muchos) | Hacia (uno) |
|---|---|
| `fact_demanda_diaria[fecha]` | `dim_fecha[fecha]` |
| `fact_flujo_gas_diario[fecha]` | `dim_fecha[fecha]` |
| `fact_flujo_gas_diario[point_key]` | `dim_punto_gas[point_key]` |
| `fact_flujo_gas_diario[direction_key]` | `dim_direccion_gas[direction_key]` |
| `fact_almacenamiento_gas_diario[fecha]` | `dim_fecha[fecha]` |
| `fact_reserva_agua_semanal[fecha]` | `dim_fecha[fecha]` |
| `fact_reserva_agua_semanal[ambito]` | `dim_ambito_agua[ambito]` |

Sin relaciones: `Sector`, `Contenido`, `Cubos`, `Barras`, `Slots Clave`, `Medidas` y `fact_frescura`.

## 2. Tablas y columnas calculadas

#### Sector

Las tres opciones del selector de sector.

```dax
Sector =
DATATABLE(
    "Sector", STRING,
    "Orden", INTEGER,
    { {"Electricidad", 1}, {"Gas natural", 2}, {"Agua", 3} }
)
```

#### Medidas

Tabla vacía que solo agrupa medidas.

```dax
Medidas =
Row("Column", BLANK())
```

#### Slots Clave

Tres filas (1, 2 y 3): una por cada clave del pie del tablero.

```dax
Slots Clave =
GENERATESERIES ( 1, 3, 1 )
```

#### Cubos

Posición y altura de cada cubo de la ilustración isométrica del banner, por sector.

```dax
Cubos =
DATATABLE (
    "Sector", STRING, "Orden", INTEGER, "I", INTEGER, "J", INTEGER, "Altura", INTEGER,
    {
        { "Electricidad", 0, 0, 0, 30 }, { "Electricidad", 1, 1, 0, 52 }, { "Electricidad", 2, 0, 1, 26 },
        { "Electricidad", 3, 2, 0, 64 }, { "Electricidad", 4, 1, 1, 88 }, { "Electricidad", 5, 0, 2, 40 },
        { "Electricidad", 6, 2, 1, 34 }, { "Electricidad", 7, 1, 2, 58 }, { "Electricidad", 8, 2, 2, 28 },
        { "Gas natural", 0, 0, 0, 44 }, { "Gas natural", 1, 1, 0, 30 }, { "Gas natural", 2, 0, 1, 60 },
        { "Gas natural", 3, 2, 0, 28 }, { "Gas natural", 4, 1, 1, 76 }, { "Gas natural", 5, 0, 2, 34 },
        { "Gas natural", 6, 2, 1, 52 }, { "Gas natural", 7, 1, 2, 24 }, { "Gas natural", 8, 2, 2, 66 },
        { "Agua", 0, 0, 0, 24 }, { "Agua", 1, 1, 0, 36 }, { "Agua", 2, 0, 1, 50 },
        { "Agua", 3, 2, 0, 38 }, { "Agua", 4, 1, 1, 58 }, { "Agua", 5, 0, 2, 72 },
        { "Agua", 6, 2, 1, 46 }, { "Agua", 7, 1, 2, 66 }, { "Agua", 8, 2, 2, 84 }
    }
)
```

#### Contenido

Textos fijos de cada sector: títulos de las tarjetas laterales e indicadores, texto del dorso al voltear e icono.

```dax
Contenido =
DATATABLE (
    "Sector", STRING, "Tipo", STRING, "Slot", INTEGER,
    "Titulo", STRING, "Dorso", STRING, "Icono", STRING,
    {
        { "Electricidad", "Lateral", 1, "Media 7 días", "", "actividad" },
        { "Electricidad", "Lateral", 2, "Máximo del periodo", "", "tendencia" },
        { "Electricidad", "Indicador", 1, "Media 30 días", "Promedio de la demanda real de los últimos 30 días cerrados. Suaviza picos de frío o calor.", "" },
        { "Electricidad", "Indicador", 2, "Mínimo del periodo", "Día con menor demanda eléctrica desde enero de 2024 (ventana de datos del proyecto).", "" },
        { "Electricidad", "Indicador", 3, "Día laborable medio", "Demanda media de lunes a viernes en los últimos 30 días.", "" },
        { "Electricidad", "Indicador", 4, "Fin de semana medio", "Demanda media de sábados y domingos; se compara con los laborables.", "" },
        { "Electricidad", "Indicador", 5, "Días sobre 700 GWh", "Días del año en curso con demanda por encima de 700 GWh: picos de frío o de calor.", "" },

        { "Gas natural", "Lateral", 1, "Entrada por GNL", "", "actividad" },
        { "Gas natural", "Lateral", 2, "Entrada por interconexiones", "", "tendencia" },
        { "Gas natural", "Indicador", 1, "Media 7 días", "Entrada física media de la última semana, sin contar consumo agregado ni punto virtual.", "" },
        { "Gas natural", "Indicador", 2, "Salida del sistema", "Gas que sale del sistema por puntos físicos hacia exportación y almacenamientos.", "" },
        { "Gas natural", "Indicador", 3, "Inyección neta", "Gas inyectado menos gas extraído en los almacenamientos subterráneos (AGSI+).", "" },
        { "Gas natural", "Indicador", 4, "Capacidad de inyección usada", "Inyección diaria dividida por la capacidad técnica de inyección de los almacenamientos.", "" },
        { "Gas natural", "Indicador", 5, "Gas almacenado", "Gas en los almacenamientos subterráneos de España frente a su volumen útil (AGSI+).", "" },

        { "Agua", "Lateral", 1, "Frente a la media de 10 años", "", "tendencia" },
        { "Agua", "Lateral", 2, "Demarcaciones bajo el 40 %", "", "actividad" },
        { "Agua", "Indicador", 1, "Hace un año", "Reserva hídrica de la misma semana del año anterior, para comparar con la actual.", "" },
        { "Agua", "Indicador", 2, "Capacidad total", "Volumen máximo que pueden almacenar los embalses incluidos en el boletín.", "" },
        { "Agua", "Indicador", 3, "Mayor reserva", "Demarcación hidrográfica con mayor porcentaje de agua embalsada.", "" },
        { "Agua", "Indicador", 4, "Menor reserva", "Demarcación hidrográfica con menor porcentaje de agua embalsada.", "" },
        { "Agua", "Indicador", 5, "Variación en 4 semanas", "Cambio de la reserva hídrica en las últimas cuatro lecturas semanales.", "" }
    }
)
```

#### Barras

Categorías del gráfico de barras de cada sector: días de la semana (Electricidad), plantas de GNL (Gas) y demarcaciones (Agua).

```dax
Barras =
VAR dias =
    SELECTCOLUMNS (
        DISTINCT ( SELECTCOLUMNS ( dim_fecha, "dia", dim_fecha[nombre_dia], "num", dim_fecha[dia_semana_num] ) ),
        "Sector", "Electricidad",
        "Etiqueta", UPPER ( LEFT ( [dia], 1 ) ) & MID ( [dia], 2, 20 ),
        "Orden", [num]
    )
VAR gnl0 =
    SELECTCOLUMNS ( FILTER ( dim_punto_gas, dim_punto_gas[tipo_punto] = "Planta GNL" ), "Etiqueta", dim_punto_gas[point_label] )
VAR gnl =
    SELECTCOLUMNS ( gnl0, "Sector", "Gas natural", "Etiqueta", [Etiqueta], "Orden", 100 + RANKX ( gnl0, [Etiqueta], , ASC, DENSE ) )
VAR agua0 =
    SELECTCOLUMNS ( dim_ambito_agua, "Etiqueta", dim_ambito_agua[ambito] )
VAR agua =
    SELECTCOLUMNS ( agua0, "Sector", "Agua", "Etiqueta", [Etiqueta], "Orden", 200 + RANKX ( agua0, [Etiqueta], , ASC, DENSE ) )
RETURN
    UNION ( dias, gnl, agua )
```

#### dim_fecha[Fecha comparada] (columna calculada)

La misma fecha (mes y día) llevada al año 2000.

```dax
Fecha comparada =
DATE(2000, MONTH(dim_fecha[fecha]), DAY(dim_fecha[fecha]))
```

#### Sector[Etiqueta] (columna calculada)

Nombre del sector con emoji.

```dax
Etiqueta =
SWITCH ( Sector[Sector], "Electricidad", "⚡ Electricidad", "Gas natural", "🔥 Gas natural", "Agua", "💧 Agua" )
```

## 3. Sector activo, colores e iconos

Tabla `Sector`. Leen el sector elegido en el selector y devuelven colores, iconos y piezas comunes al resto de medidas.

#### Clave Texto

```dax
Clave Texto =
VAR sel  = [Sector Activo]
VAR slot = SELECTEDVALUE ( 'Slots Clave'[Value] )
RETURN
SWITCH (
    TRUE (),
    sel = "Electricidad" && slot = 1,
        "La demanda del " & FORMAT ( [Elec Fecha Último Dato], "d mmm", "es-ES" ) & " fue de "
        & FORMAT ( [Elec Demanda GWh], "#,0", "es-ES" ) & " GWh, un "
        & FORMAT ( ABS ( [Elec Var Interanual %] ), "0.0%", "es-ES" )
        & IF ( [Elec Var Interanual %] >= 0, " más", " menos" ) & " que el mismo día de la semana del año anterior.",
    sel = "Electricidad" && slot = 2,
        "La demanda media de los últimos 7 días es de " & FORMAT ( [Elec Media 7d GWh], "#,0", "es-ES" ) & " GWh, un "
        & FORMAT ( ABS ( [Elec Var 7d %] ), "0.0%", "es-ES" )
        & IF ( [Elec Var 7d %] >= 0, " más", " menos" ) & " que en los 7 días anteriores.",
    sel = "Electricidad" && slot = 3,
        "El fin de semana se consume de media un "
        & FORMAT ( ABS ( 1 - DIVIDE ( [Elec Finde GWh], [Elec Laborable GWh] ) ), "0%", "es-ES" ) & " menos que entre semana.",

    sel = "Gas natural" && slot = 1,
        "España recibió " & FORMAT ( [Gas Entrada Día GWh], "#,0", "es-ES" ) & " GWh de gas el "
        & FORMAT ( [Gas Fecha Último Dato], "d mmm", "es-ES" ) & "; el "
        & FORMAT ( [Gas Cuota GNL %], "0%", "es-ES" ) & " llegó como GNL por las plantas de regasificación.",
    sel = "Gas natural" && slot = 2,
        "Los almacenamientos subterráneos están al " & FORMAT ( [Alm Lleno Último %], "0.0", "es-ES" ) & " % ("
        & FORMAT ( COALESCE ( [Alm Var Día pp], 0 ), "+0.00;-0.00", "es-ES" ) & " pp frente al día anterior).",
    sel = "Gas natural" && slot = 3,
        "La entrada media de los últimos 7 días es de " & FORMAT ( [Gas Media 7d GWh], "#,0", "es-ES" ) & " GWh, un "
        & FORMAT ( ABS ( [Gas Var 7d %] ), "0.0%", "es-ES" )
        & IF ( [Gas Var 7d %] >= 0, " más", " menos" ) & " que en los 7 días anteriores.",

    sel = "Agua" && slot = 1,
        "Los embalses están al " & FORMAT ( [Agua Reserva %], "0.0", "es-ES" ) & " %, "
        & FORMAT ( ABS ( [Agua Var Anual pp] ), "0.0", "es-ES" ) & " puntos "
        & IF ( [Agua Var Anual pp] >= 0, "por encima", "por debajo" ) & " de hace un año.",
    sel = "Agua" && slot = 2,
        "La reserva " & IF ( [Agua Vs Media10 pp] >= 0, "supera", "queda por debajo de" ) & " en "
        & FORMAT ( ABS ( [Agua Vs Media10 pp] ), "0.0", "es-ES" ) & " puntos la media de los últimos 10 años.",
    sel = "Agua" && slot = 3,
        IF ( [Agua Bajo 40 Lista] = "", "Ninguna demarcación está por debajo del 40 %.", "Por debajo del 40 %: " & [Agua Bajo 40 Lista] & "." ),
    "—"
)
```

#### Color Sector

```dax
Color Sector =
SWITCH( [Sector Activo],
    "Electricidad", "#F4B400",
    "Gas natural",  "#0EA5A4",
    "Agua",         "#3B82F6" )
```

#### Color Sector Oscuro

```dax
Color Sector Oscuro =
SWITCH( [Sector Activo],
    "Electricidad", "#7A5A00",
    "Gas natural",  "#115E59",
    "Agua",         "#1E3A8A" )
```

#### Cubos SVG

```dax
Cubos SVG =
VAR S = 22
VAR OX = 158
VAR OY = 88
VAR sel = [Sector Activo]
VAR cArr = [Pal Cubo Arriba]
VAR cIzq = [Pal Cubo Izq]
VAR cDer = [Pal Cubo Der]
RETURN
CONCATENATEX (
    FILTER ( Cubos, Cubos[Sector] = sel ),
    VAR i  = Cubos[I]
    VAR j  = Cubos[J]
    VAR h  = Cubos[Altura]
    VAR cx = OX + ( i - j ) * S
    VAR cy = OY + ( i + j ) * S / 2
    RETURN
        "<polygon points='" & ( cx - S ) & "," & ( cy - h + S / 2 ) & " " & cx & "," & ( cy - h + S ) & " " & cx & "," & ( cy + S ) & " " & ( cx - S ) & "," & ( cy + S / 2 ) & "' fill='" & cIzq & "'/>"
        & "<polygon points='" & cx & "," & ( cy - h + S ) & " " & ( cx + S ) & "," & ( cy - h + S / 2 ) & " " & ( cx + S ) & "," & ( cy + S / 2 ) & " " & cx & "," & ( cy + S ) & "' fill='" & cDer & "'/>"
        & "<polygon points='" & cx & "," & ( cy - h ) & " " & ( cx + S ) & "," & ( cy - h + S / 2 ) & " " & cx & "," & ( cy - h + S ) & " " & ( cx - S ) & "," & ( cy - h + S / 2 ) & "' fill='" & cArr & "'/>",
    "",
    Cubos[Orden], ASC
)
```

#### Frescura Etiqueta

```dax
Frescura Etiqueta =
VAR alDia =
    COUNTROWS (
        FILTER (
            fact_frescura,
            NOT ISBLANK ( fact_frescura[ultimo_dato] )
            && DATEDIFF ( fact_frescura[ultimo_dato], TODAY (), DAY ) <= fact_frescura[umbral_dias]
        )
    )
RETURN "Frescura " & alDia & "/" & COUNTROWS ( fact_frescura )
```

#### Icono Path

```dax
Icono Path =
SWITCH (
    [Sector Activo],
    "Electricidad", "M4 14a1 1 0 0 1-.78-1.63l9.9-10.2a.5.5 0 0 1 .86.46l-1.92 6.02A1 1 0 0 0 13 10h7a1 1 0 0 1 .78 1.63l-9.9 10.2a.5.5 0 0 1-.86-.46l1.92-6.02A1 1 0 0 0 11 14z",
    "Gas natural", "M8.5 14.5A2.5 2.5 0 0 0 11 12c0-1.38-.5-2-1-3-1.072-2.143-.224-4.054 2-6 .5 2.5 2 4.9 4 6.5 2 1.6 3 3.5 3 5.5a7 7 0 1 1-14 0c0-1.153.433-2.294 1-3a2.5 2.5 0 0 0 2.5 2.5z",
    "Agua", "M12 22a7 7 0 0 0 7-7c0-2-1-3.9-3-5.5s-3.5-4-4-6.5c-.5 2.5-2 4.9-4 6.5C6 11.1 5 13 5 15a7 7 0 0 0 7 7z"
)
```

#### Pal Acento

```dax
Pal Acento =
SWITCH ( [Sector Activo], "Electricidad", "#FFC933", "Gas natural", "#4FD1CF", "Agua", "#7DB2FF" )
```

#### Pal Cubo Arriba

```dax
Pal Cubo Arriba =
SWITCH ( [Sector Activo], "Electricidad", "#FFD54F", "Gas natural", "#7FE3E1", "Agua", "#9CC3FF" )
```

#### Pal Cubo Der

```dax
Pal Cubo Der =
SWITCH ( [Sector Activo], "Electricidad", "#A87500", "Gas natural", "#0A6B6A", "Agua", "#2557B5" )
```

#### Pal Cubo Izq

```dax
Pal Cubo Izq =
SWITCH ( [Sector Activo], "Electricidad", "#E0A100", "Gas natural", "#0E9594", "Agua", "#3B82F6" )
```

#### Pal Fondo

```dax
Pal Fondo =
SWITCH ( [Sector Activo], "Electricidad", "#2A2208", "Gas natural", "#082E31", "Agua", "#0F2347" )
```

#### Pal Fuerte

```dax
Pal Fuerte =
SWITCH ( [Sector Activo], "Electricidad", "#B87A00", "Gas natural", "#0E9594", "Agua", "#2563EB" )
```

#### Pal Tinta

```dax
Pal Tinta =
SWITCH ( [Sector Activo], "Electricidad", "#5C4300", "Gas natural", "#0B4F4E", "Agua", "#12306B" )
```

#### Pal Tinte

```dax
Pal Tinte =
SWITCH ( [Sector Activo], "Electricidad", "#FFF4CC", "Gas natural", "#D9F4F3", "Agua", "#E3EEFF" )
```

#### Sector Activo

```dax
Sector Activo =
SELECTEDVALUE( Sector[Sector], "Electricidad" )
```

## 4. Electricidad

Carpeta `Electricidad` de la tabla `Medidas`. Origen: `fact_demanda_diaria` (REE).

#### Demanda real GWh

```dax
Demanda real GWh =
CALCULATE ( SUM (fact_demanda_diaria[valor_gwh] ),
            dim_serie_demanda[serie] = "Demanda" )
```

#### Demanda real mismo periodo año anterior

```dax
Demanda real mismo periodo año anterior =
CALCULATE ( [Demanda real GWh], SAMEPERIODLASTYEAR ( dim_fecha[fecha] ) )
```

#### Elec Demanda GWh

```dax
Elec Demanda GWh =
VAR f = [Elec Fecha Último Dato]
RETURN CALCULATE ( [Demanda real GWh], REMOVEFILTERS ( dim_fecha ), dim_fecha[fecha] = f )
```

#### Elec Desvío Prevista %

```dax
Elec Desvío Prevista % =
DIVIDE ( [Elec Demanda GWh] - [Elec Prevista GWh], [Elec Prevista GWh] )
```

#### Elec Días Sobre 700

```dax
Elec Días Sobre 700 =
VAR f = [Elec Fecha Último Dato]
RETURN
    CALCULATE (
        COUNTROWS ( FILTER ( VALUES ( dim_fecha[fecha] ), [Demanda real GWh] > 700 ) ),
        REMOVEFILTERS ( dim_fecha ),
        dim_fecha[anio] = YEAR ( f )
    )
```

#### Elec Fecha Mínimo

```dax
Elec Fecha Mínimo =
VAR m = [Elec Mínimo GWh]
RETURN
    CALCULATE (
        MINX ( FILTER ( VALUES ( dim_fecha[fecha] ), [Demanda real GWh] = m ), dim_fecha[fecha] ),
        REMOVEFILTERS ( dim_fecha )
    )
```

#### Elec Fecha Último Dato

```dax
Elec Fecha Último Dato =
CALCULATE ( MAX ( fact_demanda_diaria[fecha] ), REMOVEFILTERS ( dim_fecha ) )
```

#### Elec Finde GWh

```dax
Elec Finde GWh =
VAR f = [Elec Fecha Último Dato]
RETURN
    CALCULATE (
        AVERAGEX ( VALUES ( dim_fecha[fecha] ), [Demanda real GWh] ),
        REMOVEFILTERS ( dim_fecha ),
        DATESINPERIOD ( dim_fecha[fecha], f, -30, DAY ),
        dim_fecha[es_fin_de_semana] = TRUE ()
    )
```

#### Elec Laborable GWh

```dax
Elec Laborable GWh =
VAR f = [Elec Fecha Último Dato]
RETURN
    CALCULATE (
        AVERAGEX ( VALUES ( dim_fecha[fecha] ), [Demanda real GWh] ),
        REMOVEFILTERS ( dim_fecha ),
        DATESINPERIOD ( dim_fecha[fecha], f, -30, DAY ),
        dim_fecha[es_fin_de_semana] = FALSE ()
    )
```

#### Elec Media 30d GWh

```dax
Elec Media 30d GWh =
VAR f = [Elec Fecha Último Dato]
RETURN
    CALCULATE (
        AVERAGEX ( VALUES ( dim_fecha[fecha] ), [Demanda real GWh] ),
        REMOVEFILTERS ( dim_fecha ),
        DATESINPERIOD ( dim_fecha[fecha], f, -30, DAY )
    )
```

#### Elec Media 7d GWh

```dax
Elec Media 7d GWh =
VAR f = [Elec Fecha Último Dato]
RETURN
    CALCULATE (
        AVERAGEX ( VALUES ( dim_fecha[fecha] ), [Demanda real GWh] ),
        REMOVEFILTERS ( dim_fecha ),
        DATESINPERIOD ( dim_fecha[fecha], f, -7, DAY )
    )
```

#### Elec Media Diaria GWh

```dax
Elec Media Diaria GWh =
AVERAGEX ( VALUES ( dim_fecha[fecha] ), [Demanda real GWh] )
```

#### Elec Media Móvil 7d GWh

```dax
Elec Media Móvil 7d GWh =
VAR d = MAX ( dim_fecha[fecha] )
RETURN
    IF (
        d <= [Elec Fecha Último Dato],
        CALCULATE (
            AVERAGEX ( VALUES ( dim_fecha[fecha] ), [Demanda real GWh] ),
            REMOVEFILTERS ( dim_fecha ),
            DATESINPERIOD ( dim_fecha[fecha], d, -7, DAY )
        )
    )
```

#### Elec Máximo GWh

```dax
Elec Máximo GWh =
CALCULATE ( MAXX ( VALUES ( dim_fecha[fecha] ), [Demanda real GWh] ), REMOVEFILTERS ( dim_fecha ) )
```

#### Elec Mínimo GWh

```dax
Elec Mínimo GWh =
CALCULATE ( MINX ( VALUES ( dim_fecha[fecha] ), [Demanda real GWh] ), REMOVEFILTERS ( dim_fecha ) )
```

#### Elec Prevista GWh

```dax
Elec Prevista GWh =
VAR f = [Elec Fecha Último Dato]
RETURN
    CALCULATE (
        SUM ( fact_demanda_diaria[valor_gwh] ),
        fact_demanda_diaria[serie] = "Demanda prevista",
        REMOVEFILTERS ( dim_fecha ),
        dim_fecha[fecha] = f
    )
```

#### Elec Var 30d %

```dax
Elec Var 30d % =
VAR f = [Elec Fecha Último Dato]
VAR anterior =
    CALCULATE (
        AVERAGEX ( VALUES ( dim_fecha[fecha] ), [Demanda real GWh] ),
        REMOVEFILTERS ( dim_fecha ),
        DATESINPERIOD ( dim_fecha[fecha], f - 30, -30, DAY )
    )
RETURN DIVIDE ( [Elec Media 30d GWh] - anterior, anterior )
```

#### Elec Var 7d %

```dax
Elec Var 7d % =
VAR f = [Elec Fecha Último Dato]
VAR anterior =
    CALCULATE (
        AVERAGEX ( VALUES ( dim_fecha[fecha] ), [Demanda real GWh] ),
        REMOVEFILTERS ( dim_fecha ),
        DATESINPERIOD ( dim_fecha[fecha], f - 7, -7, DAY )
    )
RETURN DIVIDE ( [Elec Media 7d GWh] - anterior, anterior )
```

#### Elec Var Interanual %

```dax
Elec Var Interanual % =
VAR f = [Elec Fecha Último Dato]
VAR hoy = CALCULATE ( [Demanda real GWh], REMOVEFILTERS ( dim_fecha ), dim_fecha[fecha] = f )
VAR antes = CALCULATE ( [Demanda real GWh], REMOVEFILTERS ( dim_fecha ), dim_fecha[fecha] = f - 364 )
RETURN DIVIDE ( hoy - antes, antes )
```

#### Elec Vs Media 30d %

```dax
Elec Vs Media 30d % =
DIVIDE ( [Elec Demanda GWh], [Elec Media 30d GWh] ) - 1
```

#### Variación demanda vs año anterior %

```dax
Variación demanda vs año anterior % =
DIVIDE ( [Demanda real GWh] - [Demanda real mismo periodo año anterior],
         [Demanda real mismo periodo año anterior] )
```

## 5. Gas natural y almacenamiento

Carpeta `Gas` de la tabla `Medidas`. Origen: `fact_flujo_gas_diario` (ENTSOG) y `fact_almacenamiento_gas_diario` (AGSI+).

#### Alm Cap Inyección GWh

```dax
Alm Cap Inyección GWh =
VAR f = [Alm Fecha Último Dato]
RETURN CALCULATE ( SUM ( fact_almacenamiento_gas_diario[injection_capacity_gwh] ), REMOVEFILTERS ( dim_fecha ), dim_fecha[fecha] = f )
```

#### Alm Cap Inyección Usada %

```dax
Alm Cap Inyección Usada % =
DIVIDE ( [Alm Inyección GWh], [Alm Cap Inyección GWh] )
```

#### Alm Extracción GWh

```dax
Alm Extracción GWh =
VAR f = [Alm Fecha Último Dato]
RETURN CALCULATE ( SUM ( fact_almacenamiento_gas_diario[withdrawal_gwh] ), REMOVEFILTERS ( dim_fecha ), dim_fecha[fecha] = f )
```

#### Alm Fecha Último Dato

```dax
Alm Fecha Último Dato =
CALCULATE ( MAX ( fact_almacenamiento_gas_diario[fecha] ), REMOVEFILTERS ( dim_fecha ) )
```

#### Alm Gas TWh

```dax
Alm Gas TWh =
VAR f = [Alm Fecha Último Dato]
RETURN CALCULATE ( SUM ( fact_almacenamiento_gas_diario[gas_in_storage_twh] ), REMOVEFILTERS ( dim_fecha ), dim_fecha[fecha] = f )
```

#### Alm Inyección GWh

```dax
Alm Inyección GWh =
VAR f = [Alm Fecha Último Dato]
RETURN CALCULATE ( SUM ( fact_almacenamiento_gas_diario[injection_gwh] ), REMOVEFILTERS ( dim_fecha ), dim_fecha[fecha] = f )
```

#### Alm Inyección Neta GWh

```dax
Alm Inyección Neta GWh =
[Alm Inyección GWh] - [Alm Extracción GWh]
```

#### Alm Lleno Último %

```dax
Alm Lleno Último % =
VAR f = [Alm Fecha Último Dato]
RETURN CALCULATE ( SUM ( fact_almacenamiento_gas_diario[full_pct] ), REMOVEFILTERS ( dim_fecha ), dim_fecha[fecha] = f )
```

#### Alm Var Día pp

```dax
Alm Var Día pp =
VAR f = [Alm Fecha Último Dato]
RETURN CALCULATE ( SUM ( fact_almacenamiento_gas_diario[trend_pct] ), REMOVEFILTERS ( dim_fecha ), dim_fecha[fecha] = f )
```

#### Alm Volumen Útil TWh

```dax
Alm Volumen Útil TWh =
VAR f = [Alm Fecha Último Dato]
RETURN CALCULATE ( SUM ( fact_almacenamiento_gas_diario[working_gas_volume_twh] ), REMOVEFILTERS ( dim_fecha ), dim_fecha[fecha] = f )
```

#### Almacenamiento lleno %

```dax
Almacenamiento lleno % =
CALCULATE ( AVERAGE ( fact_almacenamiento_gas_diario[full_pct] ),
            LASTDATE ( fact_almacenamiento_gas_diario[fecha] ) )
```

#### Gas Cuota GNL %

```dax
Gas Cuota GNL % =
DIVIDE ( [Gas GNL Día GWh], [Gas Entrada Día GWh] )
```

#### Gas Cuota Interconexión %

```dax
Gas Cuota Interconexión % =
DIVIDE ( [Gas Interconexión Día GWh], [Gas Entrada Día GWh] )
```

#### Gas Entrada Día GWh

```dax
Gas Entrada Día GWh =
VAR f = [Gas Fecha Último Dato]
RETURN CALCULATE ( [Gas Entrada Ref GWh], REMOVEFILTERS ( dim_fecha ), dim_fecha[fecha] = f )
```

#### Gas Entrada Ref GWh

```dax
Gas Entrada Ref GWh =
CALCULATE (
    SUM ( fact_flujo_gas_diario[valor_gwh] ),
    dim_direccion_gas[direccion] = "Entrada",
    dim_punto_gas[es_suma_fisica] = TRUE ()
)
```

#### Gas Fecha Último Dato

```dax
Gas Fecha Último Dato =
CALCULATE ( MAX ( fact_flujo_gas_diario[fecha] ), REMOVEFILTERS ( dim_fecha ) )
```

#### Gas GNL Día GWh

```dax
Gas GNL Día GWh =
VAR f = [Gas Fecha Último Dato]
RETURN
    CALCULATE (
        [Gas Entrada Ref GWh],
        REMOVEFILTERS ( dim_fecha ),
        dim_fecha[fecha] = f,
        dim_punto_gas[tipo_punto] = "Planta GNL"
    )
```

#### Gas Interconexión Día GWh

```dax
Gas Interconexión Día GWh =
VAR f = [Gas Fecha Último Dato]
RETURN
    CALCULATE (
        [Gas Entrada Ref GWh],
        REMOVEFILTERS ( dim_fecha ),
        dim_fecha[fecha] = f,
        dim_punto_gas[tipo_punto] = "Interconexión"
    )
```

#### Gas Media 7d GWh

```dax
Gas Media 7d GWh =
VAR f = [Gas Fecha Último Dato]
RETURN
    CALCULATE (
        AVERAGEX ( VALUES ( dim_fecha[fecha] ), [Gas Entrada Ref GWh] ),
        REMOVEFILTERS ( dim_fecha ),
        DATESINPERIOD ( dim_fecha[fecha], f, -7, DAY )
    )
```

#### Gas Media Diaria GWh

```dax
Gas Media Diaria GWh =
AVERAGEX ( VALUES ( dim_fecha[fecha] ), [Gas Entrada Ref GWh] )
```

#### Gas Media Móvil 7d GWh

```dax
Gas Media Móvil 7d GWh =
VAR d = MAX ( dim_fecha[fecha] )
RETURN
    IF (
        d <= [Gas Fecha Último Dato],
        CALCULATE (
            AVERAGEX ( VALUES ( dim_fecha[fecha] ), [Gas Entrada Ref GWh] ),
            REMOVEFILTERS ( dim_fecha ),
            DATESINPERIOD ( dim_fecha[fecha], d, -7, DAY )
        )
    )
```

#### Gas Máximo GWh

```dax
Gas Máximo GWh =
CALCULATE ( MAXX ( VALUES ( dim_fecha[fecha] ), [Gas Entrada Ref GWh] ), REMOVEFILTERS ( dim_fecha ) )
```

#### Gas Salida Día GWh

```dax
Gas Salida Día GWh =
VAR f = [Gas Fecha Último Dato]
RETURN CALCULATE ( [Gas Salida Ref GWh], REMOVEFILTERS ( dim_fecha ), dim_fecha[fecha] = f )
```

#### Gas Salida Media 7d GWh

```dax
Gas Salida Media 7d GWh =
VAR f = [Gas Fecha Último Dato]
RETURN
    CALCULATE (
        AVERAGEX ( VALUES ( dim_fecha[fecha] ), [Gas Salida Ref GWh] ),
        REMOVEFILTERS ( dim_fecha ),
        DATESINPERIOD ( dim_fecha[fecha], f, -7, DAY )
    )
```

#### Gas Salida Ref GWh

```dax
Gas Salida Ref GWh =
CALCULATE (
    SUM ( fact_flujo_gas_diario[valor_gwh] ),
    dim_direccion_gas[direccion] = "Salida",
    dim_punto_gas[es_suma_fisica] = TRUE ()
)
```

#### Gas Var 7d %

```dax
Gas Var 7d % =
VAR f = [Gas Fecha Último Dato]
VAR anterior =
    CALCULATE (
        AVERAGEX ( VALUES ( dim_fecha[fecha] ), [Gas Entrada Ref GWh] ),
        REMOVEFILTERS ( dim_fecha ),
        DATESINPERIOD ( dim_fecha[fecha], f - 7, -7, DAY )
    )
RETURN DIVIDE ( [Gas Media 7d GWh] - anterior, anterior )
```

## 6. Agua

Carpeta `Agua` de la tabla `Medidas`. Origen: `fact_reserva_agua_semanal` y `dim_ambito_agua` (MITECO). La media de 10 años y la reserva del año anterior llegan calculadas desde Gold.

#### Agua Almacenada hm3

```dax
Agua Almacenada hm3 =
VAR f = [Agua Fecha Último Dato]
RETURN CALCULATE ( SUM ( fact_reserva_agua_semanal[reserva_hm3] ), REMOVEFILTERS ( dim_fecha ), dim_fecha[fecha] = f )
```

#### Agua Anterior %

```dax
Agua Anterior % =
VAR f = [Agua Fecha Último Dato]
RETURN
    CALCULATE (
        DIVIDE ( SUM ( fact_reserva_agua_semanal[reserva_anio_ant_hm3] ), SUM ( fact_reserva_agua_semanal[capacidad_hm3] ) ) * 100,
        REMOVEFILTERS ( dim_fecha ), dim_fecha[fecha] = f
    )
```

#### Agua Anterior Serie %

```dax
Agua Anterior Serie % =
DIVIDE ( SUM ( fact_reserva_agua_semanal[reserva_anio_ant_hm3] ), SUM ( fact_reserva_agua_semanal[capacidad_hm3] ) ) * 100
```

#### Agua Bajo 40 Lista

```dax
Agua Bajo 40 Lista =
VAR f = [Agua Fecha Último Dato]
VAR t =
    FILTER (
        CALCULATETABLE (
            ADDCOLUMNS ( VALUES ( dim_ambito_agua[ambito] ), "@pct", [Agua Base %] ),
            REMOVEFILTERS ( dim_fecha ),
            dim_fecha[fecha] = f
        ),
        NOT ISBLANK ( [@pct] ) && [@pct] < 40
    )
RETURN
    COALESCE (
        CONCATENATEX (
            t,
            dim_ambito_agua[ambito] & " (" & FORMAT ( [@pct], "0", "es-ES" ) & " %)",
            ", ",
            [@pct], ASC
        ),
        "Ninguna"
    )
```

#### Agua Base %

```dax
Agua Base % =
DIVIDE ( SUM ( fact_reserva_agua_semanal[reserva_hm3] ), SUM ( fact_reserva_agua_semanal[capacidad_hm3] ) ) * 100
```

#### Agua Capacidad hm3

```dax
Agua Capacidad hm3 =
VAR f = [Agua Fecha Último Dato]
RETURN CALCULATE ( SUM ( fact_reserva_agua_semanal[capacidad_hm3] ), REMOVEFILTERS ( dim_fecha ), dim_fecha[fecha] = f )
```

#### Agua Demarcaciones

```dax
Agua Demarcaciones =
VAR f = [Agua Fecha Último Dato]
VAR t = FILTER ( CALCULATETABLE ( ADDCOLUMNS ( VALUES ( dim_ambito_agua[ambito] ), "@pct", [Agua Base %] ),
                 REMOVEFILTERS ( dim_fecha ), dim_fecha[fecha] = f ), NOT ISBLANK ( [@pct] ) )
RETURN COUNTROWS ( t )
```

#### Agua Demarcaciones Bajo 40

```dax
Agua Demarcaciones Bajo 40 =
VAR f = [Agua Fecha Último Dato]
VAR t = FILTER ( CALCULATETABLE ( ADDCOLUMNS ( VALUES ( dim_ambito_agua[ambito] ), "@pct", [Agua Base %] ),
                 REMOVEFILTERS ( dim_fecha ), dim_fecha[fecha] = f ), NOT ISBLANK ( [@pct] ) )
RETURN COALESCE ( COUNTROWS ( FILTER ( t, [@pct] < 40 ) ), 0 )
```

#### Agua Fecha Último Dato

```dax
Agua Fecha Último Dato =
CALCULATE ( MAX ( fact_reserva_agua_semanal[fecha] ), REMOVEFILTERS ( dim_fecha ) )
```

#### Agua Mayor %

```dax
Agua Mayor % =
VAR f = [Agua Fecha Último Dato]
VAR t = FILTER ( CALCULATETABLE ( ADDCOLUMNS ( VALUES ( dim_ambito_agua[ambito] ), "@pct", [Agua Base %] ),
                 REMOVEFILTERS ( dim_fecha ), dim_fecha[fecha] = f ), NOT ISBLANK ( [@pct] ) )
RETURN MAXX ( t, [@pct] )
```

#### Agua Mayor Nombre

```dax
Agua Mayor Nombre =
VAR f = [Agua Fecha Último Dato]
VAR t = FILTER ( CALCULATETABLE ( ADDCOLUMNS ( VALUES ( dim_ambito_agua[ambito] ), "@pct", [Agua Base %] ),
                 REMOVEFILTERS ( dim_fecha ), dim_fecha[fecha] = f ), NOT ISBLANK ( [@pct] ) )
RETURN MAXX ( TOPN ( 1, t, [@pct], DESC ), dim_ambito_agua[ambito] )
```

#### Agua Media10 %

```dax
Agua Media10 % =
VAR f = [Agua Fecha Último Dato]
RETURN
    CALCULATE (
        DIVIDE ( SUM ( fact_reserva_agua_semanal[reserva_media10_hm3] ), SUM ( fact_reserva_agua_semanal[capacidad_hm3] ) ) * 100,
        REMOVEFILTERS ( dim_fecha ), dim_fecha[fecha] = f
    )
```

#### Agua Menor %

```dax
Agua Menor % =
VAR f = [Agua Fecha Último Dato]
VAR t = FILTER ( CALCULATETABLE ( ADDCOLUMNS ( VALUES ( dim_ambito_agua[ambito] ), "@pct", [Agua Base %] ),
                 REMOVEFILTERS ( dim_fecha ), dim_fecha[fecha] = f ), NOT ISBLANK ( [@pct] ) )
RETURN MINX ( t, [@pct] )
```

#### Agua Menor Nombre

```dax
Agua Menor Nombre =
VAR f = [Agua Fecha Último Dato]
VAR t = FILTER ( CALCULATETABLE ( ADDCOLUMNS ( VALUES ( dim_ambito_agua[ambito] ), "@pct", [Agua Base %] ),
                 REMOVEFILTERS ( dim_fecha ), dim_fecha[fecha] = f ), NOT ISBLANK ( [@pct] ) )
RETURN MAXX ( TOPN ( 1, t, [@pct], ASC ), dim_ambito_agua[ambito] )
```

#### Agua Máximo %

```dax
Agua Máximo % =
CALCULATE ( MAXX ( VALUES ( dim_fecha[fecha] ), [Agua Serie %] ), REMOVEFILTERS ( dim_fecha ) )
```

#### Agua Reserva %

```dax
Agua Reserva % =
VAR f = [Agua Fecha Último Dato]
RETURN CALCULATE ( [Agua Base %], REMOVEFILTERS ( dim_fecha ), dim_fecha[fecha] = f )
```

#### Agua Serie %

```dax
Agua Serie % =
DIVIDE ( SUM ( fact_reserva_agua_semanal[reserva_hm3] ), SUM ( fact_reserva_agua_semanal[capacidad_hm3] ) ) * 100
```

#### Agua Var 1s hm3

```dax
Agua Var 1s hm3 =
VAR f = [Agua Fecha Último Dato]
VAR hoy   = CALCULATE ( SUM ( fact_reserva_agua_semanal[reserva_hm3] ), REMOVEFILTERS ( dim_fecha ), dim_fecha[fecha] = f )
VAR antes = CALCULATE ( SUM ( fact_reserva_agua_semanal[reserva_hm3] ), REMOVEFILTERS ( dim_fecha ), dim_fecha[fecha] = f - 7 )
RETURN hoy - antes
```

#### Agua Var 1s pp

```dax
Agua Var 1s pp =
VAR f = [Agua Fecha Último Dato] - 7
RETURN [Agua Reserva %] - CALCULATE ( [Agua Base %], REMOVEFILTERS ( dim_fecha ), dim_fecha[fecha] = f )
```

#### Agua Var 4s pp

```dax
Agua Var 4s pp =
VAR f = [Agua Fecha Último Dato] - 28
RETURN [Agua Reserva %] - CALCULATE ( [Agua Base %], REMOVEFILTERS ( dim_fecha ), dim_fecha[fecha] = f )
```

#### Agua Var Anual pp

```dax
Agua Var Anual pp =
[Agua Reserva %] - [Agua Anterior %]
```

#### Agua Vs Media10 pp

```dax
Agua Vs Media10 pp =
[Agua Reserva %] - [Agua Media10 %]
```

## 7. Gráficos y frescura

Medidas sin carpeta de la tabla `Medidas`: alimentan el gráfico de línea, el gráfico de barras y el botón de frescura.

#### Barra Color

```dax
Barra Color =
VAR mx =
    MAXX ( FILTER ( ALL ( Barras ), Barras[Sector] = [Sector Activo] ), [Barra Valor] )
RETURN IF ( [Barra Valor] = mx, [Pal Fuerte], "#8E9AAE" )
```

#### Barra Valor

```dax
Barra Valor =
VAR sel = [Sector Activo]
VAR sec = SELECTEDVALUE ( Barras[Sector] )
VAR etq = SELECTEDVALUE ( Barras[Etiqueta] )
RETURN
    IF (
        sec = sel,
        SWITCH (
            sel,
            "Electricidad", CALCULATE ( [Elec Media Diaria GWh], dim_fecha[nombre_dia] = LOWER ( etq ) ),
            "Gas natural",  CALCULATE ( [Gas Media Diaria GWh], dim_punto_gas[point_label] = etq, dim_punto_gas[tipo_punto] = "Planta GNL" ),
            "Agua", CALCULATE ( [Agua Reserva %], dim_ambito_agua[ambito] = etq )
        )
    )
```

#### Línea Agua

```dax
Línea Agua =
IF ( [Sector Activo] = "Agua", [Agua Serie %] )
```

#### Línea Elec

```dax
Línea Elec =
IF ( [Sector Activo] = "Electricidad", [Elec Media Diaria GWh] )
```

#### Línea Gas

```dax
Línea Gas =
IF ( [Sector Activo] = "Gas natural", [Gas Media Diaria GWh] )
```

#### Línea Ref

```dax
Línea Ref =
VAR mes = MAX ( dim_fecha[inicio_mes] )
RETURN
    CALCULATE (
        SWITCH (
            [Sector Activo],
            "Electricidad", [Elec Media Diaria GWh],
            "Gas natural",  [Gas Media Diaria GWh],
            "Agua",         [Agua Serie %]
        ),
        REMOVEFILTERS ( dim_fecha ),
        dim_fecha[inicio_mes] = EDATE ( mes, -12 )
    )
```

#### Tablas al día

```dax
Tablas al día =
CALCULATE ( COUNTROWS ( fact_frescura ), fact_frescura[al_dia] = TRUE () )
```

#### Título Barras

```dax
Título Barras =
SWITCH (
    [Sector Activo],
    "Electricidad", "Demanda media por día de la semana (GWh)",
    "Gas natural",  "Entrada media de GNL por planta (GWh)",
    "Agua",         "Reserva por demarcación (%)"
)
```

#### Título Línea

```dax
Título Línea =
SWITCH (
    [Sector Activo],
    "Electricidad", "Demanda media diaria vs mismo mes del año anterior (GWh)",
    "Gas natural",  "Entrada media diaria vs mismo mes del año anterior (GWh)",
    "Agua",         "Reserva hídrica frente a la misma semana del año anterior (%)"
)
```

#### Último día con datos almacenamiento

```dax
Último día con datos almacenamiento =
CALCULATE ( MAX ( fact_almacenamiento_gas_diario[fecha] ), ALL ( dim_fecha ) )
```

## 8. Banner

Carpeta `Banner` de la tabla `Sector`: los dos indicadores principales del banner, con su variación y su avance.

#### Banner Subtítulo

```dax
Banner Subtítulo =
SWITCH (
    [Sector Activo],
    "Electricidad", "Demanda eléctrica peninsular · último dato: " & FORMAT ( [Elec Fecha Último Dato], "d mmm yyyy", "es-ES" ),
    "Gas natural",  "Sistema gasista español · último dato: " & FORMAT ( [Gas Fecha Último Dato], "d mmm yyyy", "es-ES" ),
    "Agua",         "Embalses peninsulares · último dato: " & FORMAT ( [Agua Fecha Último Dato], "d mmm yyyy", "es-ES" )
)
```

#### Ind1 Avance

```dax
Ind1 Avance =
SWITCH (
    [Sector Activo],
    "Electricidad", DIVIDE ( [Elec Demanda GWh], [Elec Máximo GWh] ) * 100,
    "Gas natural",  DIVIDE ( [Gas Entrada Día GWh], [Gas Máximo GWh] ) * 100,
    "Agua",         DIVIDE ( [Agua Reserva %], [Agua Máximo %] ) * 100
)
```

#### Ind1 Color Var

```dax
Ind1 Color Var =
SWITCH (
    [Sector Activo],
    "Electricidad", IF ( [Elec Var Interanual %] >= 0, "#6EE7A8", "#FFA39B" ),
    "Gas natural",  IF ( [Gas Var 7d %] >= 0, "#6EE7A8", "#FFA39B" ),
    "Agua",         IF ( [Agua Var 1s pp] >= 0, "#6EE7A8", "#FFA39B" )
)
```

#### Ind1 Etiqueta

```dax
Ind1 Etiqueta =
SWITCH ( [Sector Activo], "Electricidad", "Demanda real", "Gas natural", "Entrada de gas", "Agua", "Reserva hídrica" )
```

#### Ind1 Valor

```dax
Ind1 Valor =
SWITCH (
    [Sector Activo],
    "Electricidad", FORMAT ( [Elec Demanda GWh], "#,0", "es-ES" ) & " GWh",
    "Gas natural",  FORMAT ( [Gas Entrada Día GWh], "#,0", "es-ES" ) & " GWh",
    "Agua",         FORMAT ( [Agua Reserva %], "0.0", "es-ES" ) & " %"
)
```

#### Ind1 Variación

```dax
Ind1 Variación =
SWITCH (
    [Sector Activo],
    "Electricidad", FORMAT ( [Elec Var Interanual %], "+0.0%;-0.0%", "es-ES" ) & " vs el " & FORMAT ( [Elec Fecha Último Dato] - 364, "d mmm yyyy", "es-ES" ),
    "Gas natural",  FORMAT ( [Gas Var 7d %], "+0.0%;-0.0%", "es-ES" ) & " media 7 días vs semana anterior",
    "Agua",         FORMAT ( [Agua Var 1s pp], "+0.0;-0.0", "es-ES" ) & " pp vs semana anterior"
)
```

#### Ind2 Color Var

```dax
Ind2 Color Var =
SWITCH (
    [Sector Activo],
    "Electricidad", IF ( [Elec Var 7d %] >= 0, "#6EE7A8", "#FFA39B" ),
    "Gas natural",  IF ( COALESCE ( [Alm Var Día pp], 0 ) >= 0, "#6EE7A8", "#FFA39B" ),
    "Agua",         IF ( [Agua Var 1s hm3] >= 0, "#6EE7A8", "#FFA39B" )
)
```

#### Ind2 Etiqueta

```dax
Ind2 Etiqueta =
SWITCH ( [Sector Activo], "Electricidad", "Real frente a prevista", "Gas natural", "Almacenamiento lleno", "Agua", "Agua almacenada" )
```

#### Ind2 Valor

```dax
Ind2 Valor =
SWITCH (
    [Sector Activo],
    "Electricidad", FORMAT ( [Elec Media 7d GWh], "#,0", "es-ES" ) & " GWh",
    "Gas natural",  FORMAT ( [Alm Lleno Último %], "0.0", "es-ES" ) & " %",
    "Agua",         FORMAT ( [Agua Almacenada hm3], "#,0", "es-ES" ) & " hm³"
)
```

#### Ind2 Variación

```dax
Ind2 Variación =
SWITCH (
    [Sector Activo],
    "Electricidad", FORMAT ( [Elec Var 7d %], "+0.0%;-0.0%", "es-ES" ) & " vs 7 días anteriores",
    "Gas natural",  FORMAT ( COALESCE ( [Alm Var Día pp], 0 ), "+0.00;-0.00", "es-ES" ) & " pp vs el día anterior",
    "Agua",         FORMAT ( [Agua Var 1s hm3], "+#,0;-#,0", "es-ES" ) & " hm³ vs semana anterior"
)
```

## 9. Tarjetas laterales

Carpeta `Tarjetas Visuales` de la tabla `Sector`.

#### Lateral Texto

```dax
Lateral Texto =
VAR sel  = [Sector Activo]
VAR slot = SELECTEDVALUE ( Contenido[Slot] )
RETURN
SWITCH (
    TRUE (),
    sel = "Electricidad" && slot = 1, "desde enero de 2024",
    sel = "Electricidad" && slot = 2, "frente a " & FORMAT ( [Elec Media 30d GWh], "#,0", "es-ES" ) & " GWh de media",
    sel = "Gas natural" && slot = 1,  FORMAT ( [Gas Cuota GNL %], "0%", "es-ES" ) & " de la entrada del último día",
    sel = "Gas natural" && slot = 2,  FORMAT ( [Gas Cuota Interconexión %], "0%", "es-ES" ) & " de la entrada del último día",
    sel = "Agua" && slot = 1,         "la media de 10 años es " & FORMAT ( [Agua Media10 %], "0.0", "es-ES" ) & " %",
    sel = "Agua" && slot = 2,         "de " & [Agua Demarcaciones] & " demarcaciones",
    "—"
)
```

#### Lateral Valor

```dax
Lateral Valor =
VAR sel  = [Sector Activo]
VAR slot = SELECTEDVALUE ( Contenido[Slot] )
RETURN
SWITCH (
    TRUE (),
    sel = "Electricidad" && slot = 1, FORMAT ( [Elec Máximo GWh], "#,0", "es-ES" ) & " GWh",
    sel = "Electricidad" && slot = 2, FORMAT ( [Elec Vs Media 30d %], "+0.0%;-0.0%", "es-ES" ),
    sel = "Gas natural" && slot = 1,  FORMAT ( [Gas GNL Día GWh], "#,0", "es-ES" ) & " GWh",
    sel = "Gas natural" && slot = 2,  FORMAT ( [Gas Interconexión Día GWh], "#,0", "es-ES" ) & " GWh",
    sel = "Agua" && slot = 1,         FORMAT ( [Agua Vs Media10 pp], "+0.0;-0.0", "es-ES" ) & " pp",
    sel = "Agua" && slot = 2,         FORMAT ( [Agua Demarcaciones Bajo 40], "0" ),
    "—"
)
```

## 10. Indicadores con volteo

Carpeta `Indicadores con volteo` de la tabla `Sector`.

#### Indicador Sub

```dax
Indicador Sub =
VAR sel  = [Sector Activo]
VAR slot = SELECTEDVALUE ( Contenido[Slot] )
RETURN
SWITCH (
    TRUE (),
    sel = "Electricidad" && slot = 1, FORMAT ( [Elec Var 30d %], "+0.0%;-0.0%", "es-ES" ) & " vs 30 d previos",
    sel = "Electricidad" && slot = 2, FORMAT ( [Elec Fecha Mínimo], "d mmm yyyy", "es-ES" ),
    sel = "Electricidad" && slot = 3, "últimos 30 días",
    sel = "Electricidad" && slot = 4, FORMAT ( DIVIDE ( [Elec Finde GWh], [Elec Laborable GWh] ) - 1, "+0.0%;-0.0%", "es-ES" ) & " vs laborables",
    sel = "Electricidad" && slot = 5, "en " & YEAR ( [Elec Fecha Último Dato] ),
    sel = "Gas natural" && slot = 1,  FORMAT ( [Gas Var 7d %], "+0.0%;-0.0%", "es-ES" ) & " vs 7 d previos",
    sel = "Gas natural" && slot = 2,  "media 7 días: " & FORMAT ( [Gas Salida Media 7d GWh], "#,0", "es-ES" ) & " GWh",
    sel = "Gas natural" && slot = 3,  IF ( [Alm Inyección Neta GWh] >= 0, "se inyecta más de lo que se extrae", "se extrae más de lo que se inyecta" ),
    sel = "Gas natural" && slot = 4,  "capacidad: " & FORMAT ( [Alm Cap Inyección GWh], "#,0", "es-ES" ) & " GWh/día",
    sel = "Gas natural" && slot = 5,  "de " & FORMAT ( [Alm Volumen Útil TWh], "#,0.0", "es-ES" ) & " TWh de volumen útil",
    sel = "Agua" && slot = 1,         FORMAT ( [Agua Var Anual pp], "+0.0;-0.0", "es-ES" ) & " pp interanual",
    sel = "Agua" && slot = 2,         [Agua Demarcaciones] & " demarcaciones",
    sel = "Agua" && slot = 3,         [Agua Mayor Nombre],
    sel = "Agua" && slot = 4,         [Agua Menor Nombre],
    sel = "Agua" && slot = 5,         "frente a hace 4 semanas",
    "—"
)
```

#### Indicador Valor

```dax
Indicador Valor =
VAR sel  = [Sector Activo]
VAR slot = SELECTEDVALUE ( Contenido[Slot] )
RETURN
SWITCH (
    TRUE (),
    sel = "Electricidad" && slot = 1, FORMAT ( [Elec Media 30d GWh], "#,0", "es-ES" ) & " GWh",
    sel = "Electricidad" && slot = 2, FORMAT ( [Elec Mínimo GWh], "#,0", "es-ES" ) & " GWh",
    sel = "Electricidad" && slot = 3, FORMAT ( [Elec Laborable GWh], "#,0", "es-ES" ) & " GWh",
    sel = "Electricidad" && slot = 4, FORMAT ( [Elec Finde GWh], "#,0", "es-ES" ) & " GWh",
    sel = "Electricidad" && slot = 5, FORMAT ( [Elec Días Sobre 700], "0" ),
    sel = "Gas natural" && slot = 1,  FORMAT ( [Gas Media 7d GWh], "#,0", "es-ES" ) & " GWh",
    sel = "Gas natural" && slot = 2,  FORMAT ( [Gas Salida Día GWh], "#,0", "es-ES" ) & " GWh",
    sel = "Gas natural" && slot = 3,  FORMAT ( [Alm Inyección Neta GWh], "+#,0;-#,0;0", "es-ES" ) & " GWh",
    sel = "Gas natural" && slot = 4,  FORMAT ( [Alm Cap Inyección Usada %], "0%", "es-ES" ),
    sel = "Gas natural" && slot = 5,  FORMAT ( [Alm Gas TWh], "#,0.0", "es-ES" ) & " TWh",
    sel = "Agua" && slot = 1,         FORMAT ( [Agua Anterior %], "0.0", "es-ES" ) & " %",
    sel = "Agua" && slot = 2,         FORMAT ( [Agua Capacidad hm3], "#,0", "es-ES" ) & " hm³",
    sel = "Agua" && slot = 3,         FORMAT ( [Agua Mayor %], "0", "es-ES" ) & " %",
    sel = "Agua" && slot = 4,         FORMAT ( [Agua Menor %], "0", "es-ES" ) & " %",
    sel = "Agua" && slot = 5,         FORMAT ( [Agua Var 4s pp], "+0.0;-0.0", "es-ES" ) & " pp",
    "—"
)
```

## 11. HTML

Carpeta `HTML` de la tabla `Sector`. Cada medida devuelve el HTML completo de un visual *HTML Content*.

#### HTML Banner

```dax
HTML Banner =
VAR fondo  = [Pal Fondo]
VAR acento = [Pal Acento]
VAR av     = FORMAT ( [Ind1 Avance], "0" )
VAR etiquetaFuente =
    IF (
        [Sector Activo] = "Agua",
        "<div style='font-size:11px;font-weight:600;color:" & fondo & ";background:" & acento & ";border-radius:10px;padding:3px 9px'>Fuente: MITECO · datos provisionales</div>",
        ""
    )
RETURN
"<style>
  @keyframes flotar{0%,100%{transform:translateY(0)}50%{transform:translateY(-6px)}}
  .float{animation:flotar 5s ease-in-out infinite}
  @media (prefers-reduced-motion:reduce){.float{animation:none}}
</style>
<div style='display:flex;height:168px;box-sizing:border-box;border-radius:16px;overflow:hidden;background:" & fondo & ";font-family:Segoe UI,sans-serif'>
  <div style='width:300px;flex:none'>
    <svg width='300' height='168' viewBox='0 0 300 168'>
      <g class='float' stroke='" & fondo & "' stroke-width='1' stroke-linejoin='round'>" & [Cubos SVG] & "</g>
    </svg>
  </div>
  <div style='flex:1;min-width:0;padding:14px 24px 14px 0;box-sizing:border-box;display:flex;flex-direction:column;justify-content:center;gap:10px'>
    <div>
      <div style='display:flex;align-items:center;gap:12px'>
        <div style='font-size:24px;font-weight:700;color:#fff;line-height:1.1'>" & [Sector Activo] & "</div>" & etiquetaFuente & "
      </div>
      <div style='font-size:12px;color:#C9D3E3;margin-top:2px;line-height:1.3'>" & [Banner Subtítulo] & "</div>
    </div>
    <div style='display:flex;gap:36px'>
      <div style='width:270px'>
        <div style='font-size:12px;font-weight:600;color:" & acento & ";line-height:1.3'>" & [Ind1 Etiqueta] & "</div>
        <div style='font-size:28px;font-weight:700;color:#fff;line-height:1.1'>" & [Ind1 Valor] & "</div>
        <div style='font-size:12px;color:" & [Ind1 Color Var] & ";line-height:1.3'>" & [Ind1 Variación] & "</div>
        <div style='display:flex;align-items:center;gap:8px;margin-top:6px'>
          <div style='flex:1;height:6px;border-radius:3px;background:rgba(255,255,255,.2);overflow:hidden'>
            <div style='height:6px;border-radius:3px;background:" & acento & ";width:" & av & "%'></div>
          </div>
          <div style='font-size:11px;color:#C9D3E3;line-height:1.3;white-space:nowrap'>" & av & " % del máx.</div>
        </div>
      </div>
      <div style='flex:1;min-width:0'>
        <div style='font-size:10px;font-weight:600;color:" & acento & ";line-height:1.3'>" & [Ind2 Etiqueta] & "</div>
        <div style='font-size:24px;font-weight:700;color:#fff;line-height:1.1'>" & [Ind2 Valor] & "</div>
        <div style='font-size:11px;color:" & [Ind2 Color Var] & ";line-height:1.3'>" & [Ind2 Variación] & "</div>
      </div>
    </div>
  </div>
</div>"
```

#### HTML Claves

```dax
HTML Claves =
VAR fondo  = [Pal Fondo]
VAR acento = [Pal Acento]
VAR items =
    CONCATENATEX (
        'Slots Clave',
        "<div style='height:64px;box-sizing:border-box;background:#fff;border:1px solid #E3E7EE;border-radius:12px;padding:0 16px;display:flex;align-items:center;gap:12px'>
           <div style='width:28px;height:28px;border-radius:14px;background:" & fondo & ";color:" & acento & ";font-size:13px;font-weight:700;display:flex;align-items:center;justify-content:center;flex:none'>" & 'Slots Clave'[Value] & "</div>
           <div style='font-size:13px;line-height:1.4;color:#1F2733'>" & [Clave Texto] & "</div>
         </div>",
        "",
        'Slots Clave'[Value], ASC
    )
RETURN
"<div style='display:grid;grid-template-columns:repeat(3,minmax(0,1fr));gap:12px;font-family:Segoe UI,sans-serif'>" & items & "</div>"
```

#### HTML Frescura

```dax
HTML Frescura =
VAR filas =
    CONCATENATEX (
        fact_frescura,
        VAR ult  = fact_frescura[ultimo_dato]
        VAR dias = IF ( NOT ISBLANK ( ult ), DATEDIFF ( ult, TODAY (), DAY ) )
        VAR ok   = NOT ISBLANK ( dias ) && dias <= fact_frescura[umbral_dias]
        VAR frec = UPPER ( LEFT ( fact_frescura[frecuencia], 1 ) ) & MID ( fact_frescura[frecuencia], 2, 20 )
        VAR pastilla =
            IF (
                ok,
                "<div style='display:flex;align-items:center;gap:6px;font-size:12px;font-weight:600;color:#0B5D3A;background:#DDF5E8;border-radius:12px;padding:4px 10px'><svg width='14' height='14' viewBox='0 0 24 24' fill='none' stroke='#0B5D3A' stroke-width='2.5' stroke-linecap='round' stroke-linejoin='round'><path d='M20 6 9 17l-5-5'/></svg><span>Al día · " & dias & " días</span></div>",
                "<div style='display:flex;align-items:center;gap:6px;font-size:12px;font-weight:600;color:#8A1C12;background:#FDE4E1;border-radius:12px;padding:4px 10px'><svg width='14' height='14' viewBox='0 0 24 24' fill='none' stroke='#8A1C12' stroke-width='2.5' stroke-linecap='round' stroke-linejoin='round'><path d='M18 6 6 18M6 6l12 12'/></svg><span>"
                    & IF ( ISBLANK ( dias ), "Sin dato", "Retrasada · " & dias & " días" ) & "</span></div>"
            )
        RETURN
            "<div style='display:flex;justify-content:space-between;align-items:center;padding:10px 0;border-top:1px solid #E3E7EE'>
               <div>
                 <div style='font-size:13px;font-weight:600;color:#1F2733'>" & fact_frescura[fuente] & "</div>
                 <div style='font-size:12px;color:#5B6472'>" & frec & " · último dato " & FORMAT ( fact_frescura[ultimo_dato], "d mmm yyyy", "es-ES" ) & " · margen " & fact_frescura[umbral_dias] & " días</div>
               </div>" & pastilla & "
             </div>",
        "",
        fact_frescura[tabla_silver], ASC
    )
RETURN
"<div style='padding:18px 20px;font-family:Segoe UI,sans-serif'>
   <div style='font-size:16px;font-weight:700;color:#1F2733'>Frescura de los datos</div>
   <div style='font-size:12px;color:#5B6472;margin-bottom:8px'>Último dato de cada tabla Silver y días hasta hoy</div>" & filas & "
   <div style='font-size:12px;color:#5B6472;border-top:1px solid #E3E7EE;padding-top:10px'>Una tabla está al día si su último dato no supera el margen de su frecuencia.</div>
 </div>"
```

#### HTML Indicadores

```dax
HTML Indicadores =
VAR fondo  = [Pal Fondo]
VAR acento = [Pal Acento]
VAR tinta  = [Pal Tinta]
VAR sel    = [Sector Activo]
VAR tarjetas =
    CONCATENATEX (
        FILTER ( Contenido, Contenido[Sector] = sel && Contenido[Tipo] = "Indicador" ),
        "<div class='flip'><div class='flip-in'>
           <div class='face front'>
             <div style='font-size:12px;font-weight:600;color:#5B6472'>" & Contenido[Titulo] & "</div>
             <div style='font-size:28px;font-weight:700;color:" & tinta & ";line-height:1'>" & [Indicador Valor] & "</div>
             <div style='font-size:12px;color:#5B6472'>" & [Indicador Sub] & "</div>
           </div>
           <div class='face back'>
             <div style='font-weight:700;color:" & acento & "'>" & Contenido[Titulo] & "</div>
             <div>" & Contenido[Dorso] & "</div>
           </div>
         </div></div>",
        "",
        Contenido[Slot], ASC
    )
RETURN
"<style>
  .grid{display:grid;grid-template-columns:repeat(5,minmax(0,1fr));gap:12px;font-family:Segoe UI,sans-serif}
  .flip{perspective:900px;height:112px}
  .flip-in{position:relative;width:100%;height:100%;transition:transform .6s;transform-style:preserve-3d}
  .flip:hover .flip-in{transform:rotateY(180deg)}
  .face{position:absolute;inset:0;box-sizing:border-box;border-radius:16px;backface-visibility:hidden;-webkit-backface-visibility:hidden}
  .front{background:#fff;border:1px solid #E3E7EE;padding:14px 16px;display:flex;flex-direction:column;justify-content:space-between}
  .back{background:" & fondo & ";color:#E6ECF5;padding:12px 14px;display:flex;flex-direction:column;justify-content:center;gap:4px;font-size:12px;line-height:1.45;transform:rotateY(180deg)}
  @media (prefers-reduced-motion:reduce){.flip-in{transition:none}}
</style>
<div class='grid'>" & tarjetas & "</div>"
```

#### HTML Laterales

```dax
HTML Laterales =
VAR fondo  = [Pal Fondo]
VAR acento = [Pal Acento]
VAR tinte  = [Pal Tinte]
VAR tinta  = [Pal Tinta]
VAR sel    = [Sector Activo]
-- valores calculados una sola vez, fuera del iterador
VAR v_e1 = [Elec Máximo GWh]
VAR v_e2 = [Elec Vs Media 30d %]
VAR v_e2m = [Elec Media 30d GWh]
VAR v_g1 = [Gas GNL Día GWh]
VAR v_g2 = [Gas Interconexión Día GWh]
VAR v_g1c = [Gas Cuota GNL %]
VAR v_g2c = [Gas Cuota Interconexión %]
VAR v_a1 = [Agua Vs Media10 pp]
VAR v_a1m = [Agua Media10 %]
VAR v_a2 = [Agua Demarcaciones Bajo 40]
VAR v_a2n = [Agua Demarcaciones]
VAR tarjetas =
    CONCATENATEX (
        FILTER ( Contenido, Contenido[Sector] = sel && Contenido[Tipo] = "Lateral" ),
        VAR slot = Contenido[Slot]
        VAR icono =
            IF (
                Contenido[Icono] = "tendencia",
                "M16 7h6v6M22 7l-8.5 8.5-5-5L2 17",
                "M22 12h-2.48a2 2 0 0 0-1.93 1.46l-2.35 8.36a.25.25 0 0 1-.48 0L9.24 2.18a.25.25 0 0 0-.48 0l-2.35 8.36A2 2 0 0 1 4.49 12H2"
            )
        VAR valor =
            SWITCH (
                TRUE (),
                sel = "Electricidad" && slot = 1, IF ( ISBLANK ( v_e1 ), "s/d", FORMAT ( v_e1, "#,0", "es-ES" ) & " GWh" ),
                sel = "Electricidad" && slot = 2, IF ( ISBLANK ( v_e2 ), "s/d", FORMAT ( v_e2, "+0.0%;-0.0%", "es-ES" ) ),
                sel = "Gas natural" && slot = 1,  IF ( ISBLANK ( v_g1 ), "s/d", FORMAT ( v_g1, "#,0", "es-ES" ) & " GWh" ),
                sel = "Gas natural" && slot = 2,  IF ( ISBLANK ( v_g2 ), "s/d", FORMAT ( v_g2, "#,0", "es-ES" ) & " GWh" ),
                sel = "Agua" && slot = 1,         IF ( ISBLANK ( v_a1 ), "s/d", FORMAT ( v_a1, "+0.0;-0.0", "es-ES" ) & " pp" ),
                sel = "Agua" && slot = 2,         FORMAT ( COALESCE ( v_a2, 0 ), "0" ),
                "—"
            )
        VAR texto =
            SWITCH (
                TRUE (),
                sel = "Electricidad" && slot = 1, "desde enero de 2024",
                sel = "Electricidad" && slot = 2, "frente a " & FORMAT ( v_e2m, "#,0", "es-ES" ) & " GWh de media",
                sel = "Gas natural" && slot = 1,  FORMAT ( v_g1c, "0%", "es-ES" ) & " de la entrada del último día",
                sel = "Gas natural" && slot = 2,  FORMAT ( v_g2c, "0%", "es-ES" ) & " de la entrada del último día",
                sel = "Agua" && slot = 1,         "la media de 10 años es " & FORMAT ( v_a1m, "0.0", "es-ES" ) & " %",
                sel = "Agua" && slot = 2,         "de " & v_a2n & " demarcaciones",
                "—"
            )
        RETURN
            "<div style='flex:1;min-width:0;height:168px;box-sizing:border-box;border-radius:16px;background:" & tinte & ";padding:16px;display:flex;flex-direction:column;justify-content:space-between'>
               <div style='display:flex;align-items:center;gap:10px'>
                 <div style='width:32px;height:32px;border-radius:16px;background:" & fondo & ";display:flex;align-items:center;justify-content:center;flex:none'>
                   <svg width='16' height='16' viewBox='0 0 24 24' fill='none' stroke='" & acento & "' stroke-width='2' stroke-linecap='round' stroke-linejoin='round'><path d='" & icono & "'/></svg>
                 </div>
                 <div style='font-size:12px;font-weight:700;color:" & tinta & ";line-height:1.25'>" & Contenido[Titulo] & "</div>
               </div>
               <div style='font-size:28px;font-weight:700;color:" & tinta & ";line-height:1'>" & valor & "</div>
               <div style='font-size:12px;color:" & tinta & ";line-height:1.35'>" & texto & "</div>
             </div>",
        "",
        Contenido[Slot], ASC
    )
RETURN
"<div style='display:flex;gap:20px;font-family:Segoe UI,sans-serif'>" & tarjetas & "</div>"
```

#### HTML Título

```dax
HTML Título =
VAR fondo  = [Pal Fondo]
VAR acento = [Pal Acento]
RETURN
"<div style='display:flex;align-items:center;gap:12px;height:44px;font-family:Segoe UI,sans-serif'>
  <div style='width:40px;height:40px;border-radius:12px;background:" & fondo & ";display:flex;align-items:center;justify-content:center'>
    <svg width='20' height='20' viewBox='0 0 24 24' fill='none' stroke='" & acento & "' stroke-width='2' stroke-linecap='round' stroke-linejoin='round'>
      <path d='" & [Icono Path] & "'/>
    </svg>
  </div>
  <div>
    <div style='font-size:18px;font-weight:700;line-height:1.1;color:#1F2733'>Spain Utilities Pulse</div>
    <div style='font-size:10px;color:#5B6472'>Electricidad, gas natural y agua en España</div>
  </div>
</div>"
```

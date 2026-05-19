# Tailwind_Traders_PowerBI_Capstone_Analytics
End-to-end Power BI infraestructure featuring advance DAX, python data pipelines, and a snowflake relational schema. Developed for the Microsoft Professional Certificate. 

#Introducción y Objetivos
Este proyecto consiste en la preparación, modelado y visualización de datos transaccionales de Tailwind Traders para unificar el análisis de ventas y beneficios mundiales en dólares estadounidenses (USD).

#Capacidades Demostradas
Construcción de un modelo de datos en esquema de copo de nieve. Establecimiento de relaciones jerárquicas y bidireccionales.Definición de tablas de tiempo dedicadas (Inteligencia Temporal).  Síntesis y conversión monetaria mediante lenguaje DAX.  

#Elementos y Herramientas Empleados
Microsoft Excel: Tratamiento inicial y fórmulas analíticas a nivel de fila.
Power BI Desktop: Procesos de ETL, modelado relacional y visualización.  
Python: Ingesta de tipos de cambio históricos con la librería pandas.  
Lenguaje DAX: Creación de tablas calculadas y medidas de rendimiento.  
Power BI Service: Publicación en la nube, cuadros de mando, alertas y suscripciones.  
Orígenes de Datos: Archivos Tailwind Traders Sales.xlsx, Compras y Países.

#Fases del Proyecto Paso a Paso
1. Preparación de Datos y Conexión de Fuentes (ETL)
- Cálculos en Excel (Fórmulas de fila):
  - Coste por Unidad: =E2*0.35
  - Ingresos Brutos: =E2*H2
  - Impuesto Total: =G2*H2
  - Ingresos Netos: =I2-J2
  - Beneficio: =K2-(F2*G2)
- Optimización en Power Query (Tipos de datos):
  - Configuración de claves (OrderID, PurchaseID, País ID) como números enteros.
  - Configuración de métricas financieras como números decimales fijos.
  - Auditoría de calidad con las herramientas de Calidad, Distribución y Perfil de columna (100% de validez en OrderID).
- Filtros operativos: Limpieza de la columna ReturnStatus en la tabla de Compras para dejar únicamente los registros etiquetados como "Not Returned".
- Ingesta de Divisas: Conexión mediante Script Python usando pandas para estructurar la tabla de tipos de cambio históricos (df).
- Almacenamiento: Consolidación del archivo base como Tailwind Traders Report.pbix.

2. Modelado de Datos (Esquema de Copo de Nieves)
- Relación Países ↔ Datos de Intercambio (df): Campo ID de intercambio, cardinalidad 1:1, filtro cruzado Ambos (bidireccional), activa.
- Relación Ventas ↔ Países: Campo ID de país, cardinalidad Muchos a uno (*:1), filtro cruzado Ambos, activa.Relación Compras ↔ Ventas: Campo OrderID, cardinalidad Uno a uno (1:1), filtro cruzado Ambos, activa.
- Creación de Tabla Calendario: Tabla calculada en DAX mediante funciones ADDCOLUMNS y CALENDAR (periodo 2020-2023) con campos de Año, Mes, Trimestre y Día.
- Relación Calendario ↔ Compras: Campo Fecha a Fecha de compra, cardinalidad Muchos a uno (*:1), filtro cruzado Único, activa.
- Tabla Calculada de Ventas en USD: Creación de la tabla Sales in USD con ADDCOLUMNS y RELATED para multiplicar los importes originales de la tabla Sales por el ExchangeRate de la tabla df.
- Relación Ventas en USD ↔ Ventas: Campo ID de pedido, cardinalidad Uno a uno (1:1), filtro cruzado Ambos, activa.

3. Ingeniería Analítica (Métricas DAX)
- Yearly Profit Margin: Medida que calcula el margen anual dividiendo el beneficio total entre los ingresos netos mediante la función DIVIDE (formato Porcentaje).
- Quarterly Profit Margin: Medida de inteligencia temporal con CALCULATE y DATESQTD para segmentar el margen por trimestres.
- YTD Profit Margin: Medida de acumulado anual calculada con la función TOTALYTD.
- Median Sales: Medida estadística utilizando la función MEDIAN sobre los ingresos brutos en USD.
- Optimización: Validación de las consultas en el Analizador de rendimiento asegurando latencias inferiores a 200 ms.

4. Diseño de Informes y Publicación
- Aplicación de Tema: Uso del estilo visual Parque Urbano Accesible.
- Página "Resumen de ventas":
  - Gráfico de barras agrupadas: Puntos de fidelidad por país.
  - Gráfico de columnas agrupadas: Cantidad vendida por producto.
  - Gráfico circular: Distribución de la mediana de ventas por país.
  - Gráfico de líneas: Mediana de ventas a lo largo del tiempo (con línea de tendencia).
  - Tarjetas KPI: Stock, Cantidad comprada y Mediana de ventas.
  - Segmentador: Filtro desplegable por Nombre del país.
- Página "Resumen de beneficios":
  - Gráfico de barras agrupadas: Ingresos netos por producto (orden descendente).
  - Gráfico de anillos: Margen de beneficio anual por país (porcentaje del total).
  - Gráfico de área: Margen de beneficio anual en el tiempo.
  - Tarjetas KPI: Margen de beneficio YTD e Ingresos netos USD.
  - Objeto visual de KPI: Ingresos brutos en USD con tendencia temporal.
  - Segmentador: Filtro por rango de fechas de la tabla Calendario.
- Despliegue: Publicación del archivo en la nube dentro de Mi área de trabajo en Power BI Service.
5. Operaciones y Automatización en la Nube
- Cuadro de Mando Ejecutivo: Creación del panel en la nube y anclaje (pin) de los principales gráficos y tarjetas de ambas páginas del informe.
- Optimización Móvil: Configuración del Diseño móvil reorganizando los elementos en una sola columna vertical adaptada a smartphones.

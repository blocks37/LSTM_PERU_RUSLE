# Datos de entrada

Coloca aquí `PISCOp_m.nc` o define la ruta absoluta mediante
`PISCO_NC_PATH`. Los archivos de datos se excluyen de Git para evitar publicar
información pesada o sujeta a condiciones de distribución.

El notebook espera, como mínimo:

- una variable `precipitation` expresada en milímetros por mes;
- coordenadas `longitude` y `latitude`;
- cobertura temporal mensual desde enero de 1981;
- valores válidos cerca del punto `(-76.5, -12.0)`.

Si tu NetCDF usa otros nombres de variables o coordenadas, adapta la celda de
carga y documenta la transformación. Verifica también el calendario, las
unidades, los valores faltantes y el significado de la dimensión temporal.

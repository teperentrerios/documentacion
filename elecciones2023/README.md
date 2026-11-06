# Diagramas de `teper_elecciones`

## Vistas disponibles

- `01_diagrama_entidad_relacion.puml`: vista externa y conceptual. Contiene las entidades, los atributos necesarios para el modelado y sus relaciones. Es el diagrama que se irá depurando.
- `02_modelo_relacional.puml`: vista técnica completa, con todas las tablas y columnas, nulabilidad, claves, índices y valores predeterminados.

Renderizados:

- `01_diagrama_entidad_relacion.png`
- `01_diagrama_entidad_relacion.svg`
- `02_modelo_relacional.png`
- `02_modelo_relacional.svg`

## Hallazgos

- Las tres tablas 2023 existen: `partidospoliticos_2023`, `listas_paso_2023` y `candidatos_paso_2023`.
- El modelo propuesto consolida `partidospoliticos` y `partidospoliticos_2023` en una única entidad: `partidospoliticos_2023`.
- De `partidospoliticos_2023` se eliminan `bandera_alianza`, `pasa_generales` y `cod_partido`.
- Se incorporan `color_primario` y `color_secundario`; se conserva el atributo preexistente `color`.
- `listas_paso_2023` no tiene claves foráneas declaradas; `cod_partido` es un índice.
- Del modelo propuesto de `listas_paso_2023` se eliminan `cod_estado` y `bandera_generales`.
- Para `candidatos_paso_2023` se proponen las FK `cod_lista -> listas_paso_2023.codigo`, `cod_cargo -> cargos.codigo`, `cod_departamento_candidato -> secciones.codigo` y `cod_localidad_candidato -> localidades_2023.codigo`.
- La cadena lógica validada es `partidospoliticos_2023 1:N listas_paso_2023 1:N candidatos_paso_2023`.
- Hay 1.004 listas pertenecientes a 42 partidos distintos; no existen listas huérfanas.
- Hay 15.247 candidatos distribuidos entre las 1.004 listas; no existen candidatos huérfanos.

Las relaciones de lista, cargo y localidad fueron validadas sin registros huérfanos. Antes de aplicar la FK de departamento deben corregirse 219 candidatos cuyo `cod_departamento_candidato` vale `0` y no coincide con `secciones.codigo`.

> Nota: estos cambios describen el modelo objetivo del diagrama. No se modificó la estructura de la base de datos.

## Convenciones

- `PK`: clave primaria.
- `FK`: clave foránea del modelo propuesto.
- `UQ`: restricción única.
- `IX`: índice sin restricción referencial.

Los archivos se renderizan con PlantUML mediante `plantuml archivo.puml` o cualquier extensión compatible del editor.

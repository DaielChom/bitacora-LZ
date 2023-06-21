# Refinamiento Rutinas:

## Documentacion
- La documentacion del proceso de ingestiones puede ser encontrado en la seccion de [rutinas](../rutinas/README.md) de la presetente documentacion.

## Proceso
En los refinamientos de `Historias de Usuario` de rutinas el objetivo es determinar que la informacion brindada por el usuario es suficiente para poder trabajar la HU. El proceso de refinamiento se lleva a cabo de la siguiente manera:

### Calendarizacion 2.0
Apartir de 2022-11-18 solo se recibiran nuevas rutinas bajo el esquema de calendarizacion 2.0 para estos refinamientos debemos hacer lo siquiente:

- **Leer la historia de usuario:** Se debe leer la HU y los distintos comentarios que haya para obtener el contexto de la misma. Debemos verificar qué informacion nos da el usuario respecto al repositorio y pull-request de la rutina que se trabajar, tambien debemos asegurarnos que la HU cuente con un padre.
- **logs:** 
    - Cuando se trata de modificaciones grandes o migraciones a nuevo orquestador o nuevo esquema de validacion se necesitan 3 logs. 2 de compilacion y 1 de estabilidad nuevas migraciones.
    - Para modifiaciones pequeñas son dos logs.
    - Para rutinas completamente nuevas se requerien 6 logs. 3 de compilacion y 3 de ejecucion.
    - Luego de cada modificacion solicitada deben venir logs nuevos en el PR (segun el caso). Logs que esten ya en la rama trunk (repositorio) no son validos.
- **Verificar el tipo de HU**
debemos asegurarnos si nuestra HU es una revision, una modificacion o una calendarizacion. Para la calendarizacion debemos asegurarnos que ya se haya trabajado la HU de revision, de lo contrario se etiqueta como `NO se puede trabajar`. Para revisioon o modificacion continuar con lo siquientes pasos.
- **Identificar datos:**
    - **Pull request:**
        - La HU debe contar con un PR.
        - Verificar que vaya de master a trunk.
    - **Grupo USD:** verificar grupo USD, hace parte de la documentacion y hace parte del proceso del seguimiento de rutinas, tambien es para saber a que area se pasan las incidencias.
    - **Pipeline validacion** Debemos asegurarnos que el PR ha ejecutado el pipeline de validacion `vitd_validacion_calendarizacion_LZ_CD`
    - **Revisar el MANIFEST y setup.cfg:** Las rutinas que vienen por modelo 2.0 pueden tener mapeados sus archivos en 2 partes: manifest.in (lo que estamos acostumbrados) o en el setup.cfg bajo el parámetro [options.package_data].
    - **Prefijo repositorio:** Se debe validar que el prefijo del repo de cada rutina este entre los sigueitnes. bana-, veg-, vcum-, vedp-, vrgo-, vspc-, vitd-, vsas-, vpyp-, vnc-, vsuf-, veyg-, vmdo-, bani-
- **Comentar HU:** Se debe dejar un comentario en la HU indicando los hallazgos del refinamiento.


### Calendarizacion 1.0

- **Leer la historia de usuario:** Se debe leer la HU y los distintos comentarios que haya para obtener el contexto de la misma. Debemos verificar qué informacion nos da el usuario respecto al repositorio y pull-request de la rutina que se trabajar, tambien debemos asegurarnos que la HU cuente con un padre.
- **Verificar tipo de HU:**

si es una rutina tradicional o virtual. las tradicionales entregan codigo compleoto, las virtuqales entrgan pr. 
si es revision
si es modificacion
si es calendarizacion


- **Identificar datos:**
    - **Logs:** verificar cantidad de logs, almenos 10 logs, los logs con ejecucion exitosa. Verificar que no hayan errores, algunos errores son tolerables. que los logs sean de almenos 3 dias y que hayan 2 horas entre ellos. esto se puede verificar con el nombre de los logs ya que el nombre es la fecha y la hora de ejecucion. verificar que los lofs son despues del pr.
    - **Grupo USD:** verificar grupo USD, hace parte de la documentacion y hace parte del proceso del seguimiento de rutinas, tambien es para saber a que area se pasan las incidencias.
    - **Pipeline de validacion:** El stage de pipeline de validacion debe estar ejecutado (En verde).
    - **Revision codigo:** revisar dsn, datos quemados, manifest, 
        - Revisar que el nombre del paquete este corecto en el setup.py
        - Revisar dsn en el archivo config. Debe ser impala-virtual-prd


# TODO

verificar pull request si se trata de rutionas virtuales o el codigo si se trata de tradicionales.
Si entregan el repositorio buscar en los pr de  dicho repo al usiaro, el pr debe estar activo y listo para aprobar, echarle un ojo por encima de los archivos del pr, verificar como minimo el config que tenga el dsn bien.

verificar prefijo de repo



si es calendarizacion veriicar la hora y la periodicidad
- En el setup.py esta especiicado que orquestador se usa.



LISTA DE PREFIJOS
bana-
veg-
vcum-
vedp-
vrgo-
vspc-
vitd-
vsas-
vpyp-
vnc-
vsuf-
veyg-
vmdo-
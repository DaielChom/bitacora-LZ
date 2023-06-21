# Uso de del script para replicación de datos   a  s3

Script de Python3 para Cargar Datos de HDFS a S3

Este es un script de Python3 que carga datos desde HDFS a S3 utilizando la librería Hadoop distcp. Además, refresca los datos y recrea tablas en CDP PC. También puede cargar particiones parciales basadas en un campo en la tabla de entrada. El script lee los parámetros de entrada desde la línea de comandos y un archivo de configuración, y utiliza las librerías Boto3 y tqdm. Utiliza multithreading para copiar archivos a S3 en paralelo, y registra el progreso en un archivo. El script también realiza la gestión de errores y validación.

# Funcionamiento

El script funciona de la siguiente manera:

Lee los parámetros de entrada desde la línea de comandos y el archivo de configuración. Configura la conexión a HDFS y S3. Obtiene la lista de archivos a copiar. Divide la lista de archivos en grupos para el procesamiento en paralelo. Para cada grupo de archivos:

```
  Copia los archivos a S3 utilizando multithreading.
  Registra el progreso en un archivo de registro.
  Carga los archivos en CDP PC y recrea las tablas.
  Refresca los datos en CDP PC.
```

Realiza la gestión de errores y validación.

Para utilizar el script, se deben proporcionar los siguientes parámetros de entrada desde la línea de comandos:

- El directorio de entrada en HDFS.
- El directorio de salida en S3.
- El archivo de configuración.
- El número de subprocesos para el procesamiento en paralelo.
- El campo para la carga de particiones parciales.

Además, el archivo de configuración debe contener las siguientes variables:

- El nombre del bucket de S3.
- El ID de la cuenta de CDP PC.
- El nombre del clúster de CDP PC.

El script puede ser ejecutado mediante el siguiente comando: Todos los scripts tienen tres campos obligatorios, los cuales son: alias/aws/s3, que siempre debe estar porque se encarga de cargar las configuraciones de S3, ProcesoSQP, que debe ser igual al insertado en la tabla insumos_s3 en proceso SQP, y usuario_servicio, que es el usuario de servicio habilitado para conectarnos a CDP.

1- Para cargar datos históricos, es decir, cargar toda la historia de la tabla, se deben cargar X cantidad de archivos, excepto los correspondientes a los últimos X días.
Nota: los parámetros son: 10 dynamic 10.
El primer 10 es el valor del número de hilos a nivel de hadoop distcp. Este valor debe ser un número entero entre 1 y 10, aunque se recomienda utilizar como máximo 3 hilos.
El segundo 10 es el valor del número de threads a nivel de servidor, es decir, la cantidad de hadoop distcp que se van a ejecutar al mismo tiempo. Se recomienda utilizar un valor entero entre 1 y 5 para evitar saturar el servidor.
Finalmente, el valor dynamic se puede dejar por defecto.

- El proceso SQP se inserta en la tabla resultados.insumo_s3.
- Se especifica el usuario con el que se realiza la carga mediante la opción "-u".
- La cantidad de mappers en HDFS se indica con la opción "-m".
- Se utiliza la estrategia "dynamic".
- La opción "-t" indica la cantidad de hilos con los cuales se realizará la concurrencia de las cargas.
- La opción "-n" especifica la cantidad de máquinas en las que se realizarán las cargas.
- Se puede elegir el método de carga con la opción "-me": "ParByPar" para particionar por partición, o "FiByFi" para cargar archivo por archivo.

```python
python3 copy_to_s3_V2_FULL.py alias/aws/s3 -p SQPDIRTN004 -u srv_svccdp03 -m 10 -s dynamic -t 10 -n m001 m002 -me ParByPar
python3 copy_to_s3_V2_FULL.py alias/aws/s3 -p SQPDIRTN004 -u srv_svccdp03 -m 10 -s dynamic -t 10 -n m001 m002 -me FiByFi

```

2- Para carga de su ultima partición es decir la partición mas nueva existente en la tabla :

```python

python3 copy_to_s3_V2.py  alias/aws/s3  SQPDI1234DCPVICT srv_svccdp03

```

3- Para cargar tablas sin particiones hemos de usar :

```python

python3 copy_to_s3_V2_without.py  alias/aws/s3  SQP2485NOPARTI srv_svccdp03

```

La ruta de los logs se encuentra en la `ruta base + ProductizarPy3\ReplicacionS3\TMP_FILES\logs` allí verás archivos con el nombre replicación_s3_fecha.log

Para obtener más información, póngase en contacto con vmtorres.

# Paso a Paso scripts copy to S3

### 1. Refinamiento previo al trabajo

Para indicar si una HU se puede trabajar o no

```python
Se debe de hacer un show partitions de la tabla
La tabla debe de estar particionada minimamente por un campo
Para poder hacer uso de los scripts

copy_to_s3_V2_FULL.py
copy_to_s3_V2.py
```

En caso contrario  de se debe de usar el  el copy_to_s3_V2_without.py

La HU debe indicar que se realizará una carga histórica de datos con una periodicidad y una hora determinadas. Es importante mencionar que se realizará una carga de toda la historia inicialmente y se continuará cargando la última partición de manera regular.

### 2. Comienzo Trabajo de la HU:

Una vez identificadas las tablas y sus particiones, se procede a la inserción en la tabla `resultados.insumos_s3` en PRD.

2.1 Para adquirir el último ID, utiliza la siguiente consulta: `select * from resultados.insumos_s3 order by id desc;`

2.2 Una vez obtenido el ID, realiza la inserción de la siguiente manera:

`insert into resultados.insumos_s3 values(id, fecha_inserción, BasesDatos, NombreTabla, HU#ID, 'SQP+Periodicidad+DCP+last4NumerosHU', 'aws_cdp', 'cdp-bcl-prd', 'datos', ParticionTabla, true, cantidadDiasConservarEnTierra);`

Después de generar la inserción en la base de datos, estás listo para realizar las ejecuciones con el apoyo de infraestructura.

Ejemplo de apoyo:

Ambiente PRD
Usuario: svchad02
Servidor: edge001
Ruta:

`cd /opt/Cloud/ProductizarPy3/`

Comandos:

1. `nohup python3 copy_to_s3_V2_FULL.py alias/aws/s3 -p SQPDI691DCP -u srv_svccdp03 -m 4 -s dynamic -t 4 -n e001 -me ParByPar &`

Para cargar históricos **sin malla**, utiliza las indicaciones del comando copy_to_s3_V2_FULL.

Para consultar la ruta de los registros, dirígete a: **/opt/Cloud/ProductizarPy3/ReplicacionS3/TMP_FILES/logs**. El archivo suele ser el último y tiene la siguiente taxonomía: replicacións3_fechalarga.log.

Si la ejecución se completó con éxito, el registro mostrará algo similar.

![Untitled](Uso%20de%20del%20script%20para%20replicacio%CC%81n%20de%20datos%20a%20s3%2040479772623a4c29b55ddfe04091df35/Untitled.png)

En caso contrario

![Untitled](Uso%20de%20del%20script%20para%20replicacio%CC%81n%20de%20datos%20a%20s3%2040479772623a4c29b55ddfe04091df35/Untitled%201.png)

De forma adicional pueden consultar el la tabla resultados.datalake_informacion_lz

Al usar esta consulta:
`select state_of_charge,upload_log,last_instruction from resultados.data_lake_informacion_lz where table_name_onpremise ="NombreTabla";`

Correcta finalización

![Untitled](Uso%20de%20del%20script%20para%20replicacio%CC%81n%20de%20datos%20a%20s3%2040479772623a4c29b55ddfe04091df35/Untitled%202.png)

Finalización incorrecta

![Untitled](Uso%20de%20del%20script%20para%20replicacio%CC%81n%20de%20datos%20a%20s3%2040479772623a4c29b55ddfe04091df35/Untitled%203.png)

Para casos en los que se requiera cargar datos históricos **con malla**:

Se debe de tener muy en cuenta la periodicidad, hora de ejecución  y si este tendrá prerrequisito

Antes de realizar la  inscripción en malla es muy importante haber realizado una ejecución controlada de forma exitosa 

ya fuese con el script  `copy_to_s3_V2_Full.py`, `copy_to_S3_V2.py` o `copy_to_s3_V2_without.py`

Luego de validar de forma exitosa la ejecución controlada del script

Se debe de reservar el paquete en PRD:

Para lograr esto, primero consultamos los últimos paquetes en inventarios LZ mediante esta consulta:

```sql
select cast(REGEXP_REPLACE(package,'[^0-9]+', "") as int) as consecutivo, package, proceso, count(*)
from resultados.inventario_lz
group by 1,2,3 order by 1 DESC limit 10;

```

Después de encontrar el consecutivo, se procede a realizar la inserción en PDN en la tabla resultados.inventario_lz. Por ejemplo, así:

```
Insert into resultados.inventario_lz (package,table_name,schema_table_prod,semilla,proceso,origen_fuente,hive_database,short_name,data_folder,workflow,subdomine,periodicidad,descripcion,solicita_ingestion,dia_ejecucion,hora_ejecucion,prerrequisito) values ('SQOOPDI2579','prd_act_mst_tc_cuentas','resultados_bam_vdd','','SQPDI2779DCPVDD','DCP','resultados_bam_vdd','SQP','cd /opt/Cloud/ProductizarPy3/ && python3 copy_to_s3_V2.py alias/aws/s3 SPQDIDCP6706 srv_svccdp03','cd /opt/Cloud/ProductizarPy3/ && python3 copy_to_s3_V2.py alias/aws/s3 SPQDIDCP6706 srv_svccdp03','Entorno Otros EVC Cloudera Data Platform FC BIGDATA','Mensual','Proceso que emplea Hadoop disctp para cargar la ultima partición de las tablas:resultados_bam_vdd.prd_act_mst_tc_cuentas','Vanessa Castrillon Monsalve','Diaira','10 .AM','N/A')

```

Donde:

- package: nombre del paquete con el consecutivo generado SQOOP + periodicidad + consecutivo.
- table_name : nombre de la tabla en Producción.
- schema_table_prod : Nombre de la base de datos en Producción.
- semilla: No aplica. Puede dejarse vacío, es decir: ''.
- Proceso: Para matricularse en la malla, se debe generar el proceso siguiendo estas convenciones:
    
    ![Uso%20de%20del%20script%20para%20replicacio%CC%81n%20de%20datos%20a%20s3%2040479772623a4c29b55ddfe04091df35/Untitled%204.png](Uso%20de%20del%20script%20para%20replicacio%CC%81n%20de%20datos%20a%20s3%2040479772623a4c29b55ddfe04091df35/Untitled%204.png)
    
- origen_fuente: DCP
- hive_database: Nombre de la base de datos en la LZ
- short_name: SQP
- data_folder: Comando a matricular en malla
- workflow: Comando a matricular en malla
- subdomine: Entorno Otros EVC Cloudera Data Platform FC BIGDATA
- periodicidad: Mensual, diaria, semanal, etc.
- descripcion: Breve descripción de lo que se hará
- solicita_ingestion: Dueño de la HU
- dia_ejecucion: Día en que se realiza la ejecución del proceso
- hora_ejecucion: Hora de ejecución del proceso
- prerrequisito: Si tiene prerrequisito

Una vez hecho el insert en inventarios LZ, crear la documentación en la WIKI de DEVOPS.

[Procesos_SQP - Overview (visualstudio.com)](https://grupobancolombia.visualstudio.com/Vicepresidencia%20Servicios%20de%20Tecnolog%C3%ADa/_wiki/wikis/Vicepresidencia%20Servicios%20de%20Tecnolog%C3%ADa.wiki/43592/Procesos_SQP)

Se adjunta una documentación de ejemplo para que sirva de guía:
[SQPDI2435DCPBANAV - Overview (visualstudio.com)](https://grupobancolombia.visualstudio.com/Vicepresidencia%20Servicios%20de%20Tecnolog%C3%ADa/_wiki/wikis/Vicepresidencia%20Servicios%20de%20Tecnolog%C3%ADa.wiki/60714/SQPDI2435DCPBANAV)

Una vez que hayas realizado la documentación y estés listo para realizar la inscripción en la malla, debes seguir estos pasos:

En el apartado de comandos, escribe lo siguiente:
`cd /opt/Cloud/ProductizarPy3/ && python3 copy_to_s3_V2.py alias/aws/s3 SPQDIDCP6706 srv_svccdp03`

Tener en cuenta que el comando varia dependiendo del del tipo de replicación o carga a realizar.

¡Felicidades! 🍰🎉 Si has llegado hasta aquí, debes sentirte orgulloso de haber completado de manera exitosa una HU de replicación de datos a CDP. ¡Buen trabajo!
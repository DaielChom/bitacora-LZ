Debemos de indentificar que los archivos existan en S3., esto se hace en el refinanmiento(equipo interno con accesos a PDN)


Se debe de buscar el nombre del paquete  en donde existe la tabla

nos vamos para el asistente web
y en ejecutar programa 


## Ejecución fecha especifica​

- Ubicarse en la ruta para DEV Y UAT:  `/opt/gitDevops/scripts/s3_descargaPlano` 

`python descargaS3.py nombreBucket nombre_s3_workflows_list.txt /ubicacionAmbiente/properties.cfg año mes día​`
​
- Ejemplo:​

`cd /opt/gitDevops/scripts/s3_descargaPlano && python descargaS3.py aw1003001-productizaranalitica-banitsmo-dev SQOOPDI1074_s3_workflows_list.txt /opt/gitDevops/s3_workflows/SQOOPDI1074/properties.cfg 2020 06 02​`

## Ejecución día Parametrizable

- Ubicarse en la ruta: “`/opt/gitDevops/scripts/s3_descargaPlano`”
​
`python descargaS3.py nombreBucket nombre_s3_workflows_list.txt /ubicacionAmbiente/properties.cfg 0 0 0 ​`

- Ejemplo: ​
`cd /opt/gitDevops/scripts/s3_descargaPlano && python descargaS3.py aw1003001-productizaranalitica-banitsmo-dev SQOOPDI1074_s3_workflows_list.txt /opt/gitDevops/s3_workflows/SQOOPDI1074/properties.cfg 0 0 1`



Ruta de los paquetes DEV:

**/home/carolond/AW1003001_BigDataCompany/s3_workflows**

Ruta de los paquetes PRD:

/home/svchad02/s3_workflows

1. Revisar que los archivos esten en el bucket
2. Revisar en la ruta de s3 workflows donde estan las tablas realmente
3. Pasar las cosas de la nube al servidor
    1. Quedan en el servidor en la siguiente ruta:/home/svchad02/descargas_csv_s3
    2. Comando ejemplo:
        1. Ruta:cd /home/svchad02/scripts/s3_descargaPlano
        2. Comando: nohup python descargaS3.py  <Bucket> <Workflowlist> <ruta al archivo de properties> <Año Mes Dia> &
        3. Ejemplo:

nohup python descargaS3.py productizar-pdn-s3-habilitador-ingesta-banistmo SQOOPDI1591_s3_workflows_list.txt /home/svchad02/s3_workflows/SQOOPDI1591/properties.cfg 2022 02 03 &

1. Y despues se tira el comando de ingestion normal

nohup python3 submit.py SQOOPDI1591-getContext.py FF_S_Bani_Canales_SVE_SVE_HMON &



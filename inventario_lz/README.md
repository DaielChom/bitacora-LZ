## Rutinas

- **Reservar paquete Hue**
si la rutina es una modificacion no hay que reservar paquete ni llevar al autoinventory ni generar la documentacion. Sin embargo es buna practica verifica que la rutina se esta ejecutando correctamente en prd a traves de control M. De lo contrario si se trata de una calendarizacion hay que registrar la nueva rutina. Para ello lo que se hara es registrar la rutina en el inventario de la LZ y crear documentacion.

    - **Determinar nombre de proceso**: Buscar los ultimos 10 para ver cual es el ultimo para crear uno nuevo. Ejecutamos la siguiente consulta en HUE-DEV.

            $ select cast(REGEXP_REPLACE(package,'[^0-9]+', "") as int) as consecutivo, package, proceso, count(*) from resultados.inventario_lz group by 1,2,3 order by 1 DESC limit 10;

        Tambien se podria ejecutar desde el servidor de DEV usando el comando

            $ impala-shell -i $STR_CNX_IMPALA -k -q "<query>" --ssl

        Verificamos cual es el ultimo numero consecutivo y tomamos el siguiente para nuestro proceso. Debemos tener muy ecuenta el nombre del paquete sqp y el nombre del proceso. son diferentes.
        
            paquete sqp => SQOOPDI1937 ->  SQOOP + periodicidad + ultimo consecutivo + 1
            proceso sqp => SQPDI1937RTNCSD -> SQP + periodicidad + ultimo consecutivo + 1 + RTN + Aplicativo

        Podemos ver rutinas de ejemplo con la siguiente query.
            
            $ SELECT package,table_name,semilla,proceso,origen_fuente,data_folder,periodicidad,descripcion,solicita_ingestion,dia_ejecucion,hora_ejecucion,prerrequisito FROM resultados.inventario_lz WHERE origen_fuente='Rutina' LIMIT 10
    
    - **Separar paquete:** Una vez tengamos el nombre del paquete y proceso definidos vamos aregistrar la rutina en el autonventory. la idea es ejecutar la sigueinte consuslta reemplazando los campos requerdos.
    
            $ INSERT INTO resultados.Inventario_LZ (package,table_name,schema_table_prod,semilla,proceso,origen_fuente,hive_database,short_name,data_folder,workflow,subdomine,periodicidad,descripcion,solicita_ingestion,dia_ejecucion,hora_ejecucion,prerrequisito) VALUES ('<paquete>','','','<nombre-rutina>','<proceso>','Rutina','<zona-resultados>','<aplicativo>','<directorio-rutina>','<comando-ejecucion>','<grupo-USD>','<periodicidad>','<descripcion>','<usuario-hu>','<dia-ejecucion>','<hora-ejecucion>','<prerrequisto>');

        Ejemplo parametrios:
        |parametro|ejemplo|
        |-|-|
        |`<paquete>`|SQPSE2278|
        |`<nombre-rutina>`|Rutina de Contactabilidad|
        |`<grupo-USD>`|Jurídico Analítico Personas Enriquecimiento y Monitoreo.|
        |`<proceso>`|SQPSE2278RTNCPN|
        |`<directorio-rutina>`|/avirtual/resultados_vppc_dir_estrat_clte_pers/contactabilidad_pn/|
        |`<periodicidad>`|Semanal|
        |`<descripcion>`|Calendarizar el proceso de Contactabilidad para su correcto cargue semanal, en la LZ en la zona de resultados resultados_vppc_dir_estrat_clte_pers.|
        |`<usuario-hu>`|Santiago Restrepo Yepes|
        |`<dia-ejecucion>`|Martes|
        |`<hora-ejecucion>`|1:00pm|
        |`<prerrequisto>`|N/A|
        |`<aplicativo>`|CPN|
        |`<comando-ejecucion>`|python3 /home/svchad02/scripts/pyEnvs/runPyEnv.py --resultDir resultados_vppc_dir_estrat_clte_pers --pyEnvPkg contactabilidad_pn --pySitePck contactabilidad_pn|

        podemos verificar el registro volviendo aconsultar los ultimos 10 procesos.

- **Inventario LZ PRD:** Se debe enviar un correo a infra para agregar la rutina al inventario en PRD.

      Solicito su apoyo ejecutando los siguientes comandos.

          Buenas tardes, solicito su apoyo ejecutando el siguiente comando.

          Usuario: svchad02
          Ambiente: PRD 
          Servidor: SBMDEBLZe001

          impala-shell -i $OOZIE_STR_CNX_IMPALA -k -q "<insert-query>" --ssl

# Ingestiones
- **state:** Building
- **last_update:** 2022-08-19

Una ingestion consiste al proceso de ingresar datos provenientes de distitnas partes del banco al servidor principal, el cual es un HDFS que utiliza hadoop, sqoop y otras herramientas de sistemas distribuidos (Cloudera). En entornos de produccion estas ingestiones se realizan de manera periodica y tiene una serie de scripts para su ejecucion. Las `Hisotiras de Usuario` (HU) de nuevos procesos de ingestiones consistenen generar dichos scripts para que el proceso de ingestion se pueda llevar de manera automatica. Para la correcta elaboracion de este proceso se generan tablas temporales con las cuales realizar las pruebas de ingestion en entornos de desarrollo y local.

El proceso para generar scripts, la creacion de tablas temporales y otros procesos para completar HU de ingestiones se listan acontinuacion.

## Documentación
-	Existe una documentación referente a este proceso en los archivos del equipo **Células LZ** de *Teams* en *`Desarrollo/CDH – BigDataCompany/Bases de datos temporales/`* o en *`test/CDH – BigDataCompany/Bases de datos temporales/`*
-	Hay una serie de videos donde se da capacitación sobre ingestión de tablas temporales, revisar siempre el mas reciente. Dichos videos están en en los archivos del equipo **Células LZ** de *Teams* en *`General/Gestion Historias Usuario/Ingestiones/`*

**Nota:** Al parecer hay uno que otro markdown rotando entre los desarrolladores con explicaciones de este proceso de ingetsiones (Mejor explicados que los archivos en la documentacion mencionada).

## Proceso
0. **Crear tareas:** En todas las `HU` que se esten trabajando, independientemetne del tipo, se deben crear tareas asociadas, esto para llevar un control del estado de cada `HU` y como requisito de algunos de los pasos a la hora de llevar a produccion. Se recomienda crear las tareas de `Desarollo` y `Salida a produccion`, sin embargo, queda a criterio del desarollador que tareas crear. No olvidar cambiar el estado de las tareas cuando sea necesario.
1. **Revisar HU:** Se debe verificar que la Historia de Usuario es trabajable y descargar las plantillas adjuntas. Estas plantillas contienen la informacion de las tablas a las cuales se les realizara la ingestion. Debemos tomar nota del codigo de la historia de usuario ya que sera util durante todo el proceso. Estar muy atento del prerequisito que puedan tener las tablas ya que si varias tablas tienen distintos prerequisitos se debe crear un paquete por cada prerequisito, tambien sucede si las tablas tienen disintas horas o periodicidad de ingestion.

    - Si en la HU se especifica que es una **INGESTION DE UNA UNICA VEZ** ver la siguiente [guia](./ingestion_unica_vez.md) ya que el proceso es totalmente diferente. Igualmente buscar apoyo en alguien del equipo que ya alla trabajado con este tipo de ingestiones, ya que no son muy comunes.

    - Si la HU es de modificacion debemos identificar que cambios son necesarios de realizar. Posiblemente seran modificaciones en el paquete. En ese caso la HU consistira en:
        - Crear una nueva rama.
        - Hacer los cambios solicitados.
        - Ejecutar el paquete y las pruebas automatizadas.
        - Generar un PR y completar el release.
        - Verificar que la documentacion este bien diligenciada antes de llevar a certificacion o PR.
        - Si en la HU de modificacion no se solicita algun cambio en la ejecucion del proceso en malla no hay necesidad de hacer inscripcion.
    
        En otras palabras no se debe hacer el paso de generar el paquete, ya que este existe.
    
    - Si la ingestion de intraday debe haber informacion del inicio y fin del proceso cada dia.
    - Tener en cuenta que la LZ NO HACE PERSONALIZACIONES a nivel del paquete, por eso cualquier modificacion del where condition en el paquete no esta permitida. Hay cambios en el paquete que si estan permitidos como los cambios de mappers.
    - **Existencia:** revisar que la tabla ya no se este ingestando. Buscar nombre, esquema, aplicativo, fuente, etc. La idea es que dos flujos no queden con el mismo nombre, ya que generaria problemas.

2. **Preparar datos:** Se deben realziar algunas modificaciones en la plantilla descargada. Tambien es necesario tomar nota de alguna informacion de la plantilla que sera util para el proceso de ingestion. 

    - Cambiar el nombre de la plantilla con el siguiente formato. `HU<HU_id>.xlsm`. `Ejemplo: HU2390426.xlsm`
    - Cambiar el nombre de las hojas 
        - *"info general fuente"* a *"general"*
        - *"Metadata Tecnica Tabla BD"* a *"metadata"*
    - Desproteger las hojas con la contraseña *"bigdatacompany"*
    - Eliminar los macros *general* y *metadata* del excel.
        - ALT + F11 (ingresar visual basic)
        - Seleccionar todo el macro,eliminar y guardar.
        
    **Nota:** En vez de realizar los cambios en la plantilla se aconseja directamenete cambiar la plantilla utilizando un archivo valido y copiar los datos de la plantilla entregada por el usuario a la plantilla nueva. 
    - Al copiar los datos tener muy en cuenta que se copien de manera correcta en los campos correspondientes. Si sale alguna ventana emergente darle ***Si a todo***
    - Verificar que los campos de cantidad de caracteres en nombre de tabla y alias esten bien diligenciados y no esten vacios, de lo contrario se podria causar un error los estados del release de autoinventario.
    - La nueva plantilla debe quedar con el formato indicado en los item anteriores.
    - El valor en las columnas de esquema debe ir sin punto final.
    

    - En algun archivo tomar notas de la siguiente informacion:
        |Parametro|Identificador|Fuente|Ejemplo|
        |-|-|-|-|
        |NUMERO HU|`<HU_id>`|-|3224139
        |Aplicativo|`<servicio_aplicativo>`, `<sistema_fuente>`|Plantilla Excel|SULEAS
        |NOMBRE SUBDOMINIO|`<subdominio>`|Historia de Usuario|Leasing_DB2
        |ESQUEMA|`<schema>`|Plantilla Excel|LSD
        |NOMBRE TABLAS EN PRD|`<table_name>`|Plantilla Excel|LSLOCARRBF
        |BD DESTINO|`<bd_destino>`,`<db_en_hive>`|Plantilla Excel|S_Productos
        |NUMERO TABLAS|-|-|-|
        |MOTOR_DB|`<motor_db>`|Historia de Usuario, Plantilla Excel|DB2
        |TIPO DE CARGA|`<tipo_carga>`|Plantilla Excel|FULL/Incremental
        |PERIODICIDAD|`<periodicidad_carga>`|Plantilla Excel|DI/ME/AN
        |HORA|`<HoraPreRequisito>`|Plantilla Excel|07:00:am
        |PAQUETE|`<paquete>`|py_workflows, Hisotoria de Usuario|Tomado de el inventario de fuentes|
        |COLUMNONEBYONE|`<>`|*Si requiere campos especificos a traer, debe especificarse.*|SI/NO
        |PREREQUISITO|`<pre-requisito>`| Plantilla Excel |

        **Notas:**
        - Si se trata de una tabla nueva puede que no exista el paquete. La idea de la ingestion es generar dicho paquete.
        - El aplicativo u otros campos en la HU puede venir con tildes o con la primera en mayuscula, en tal caso consultar con el equipo cual deberia ser la mejor forma de tomar dicho dato con caracteres especiales. Ejemplo, para un `Aplicativo` que en la plantilla venia como ***Préstamos*** se toma como ***prestamos***. 
        - El aplicativo debe ser una serie de caracteres de 3 letras, esto debido al nombramiento del proceso. En caso de que no se cuente con un aplicativo con estas caracteristicas, hablar con el usuario.
        - El prerrequisito lo necesitaremos para cuando hagamos la inscripcion en malla.

3. **Preparar repositorio.** El desarrollo de la ingestion se lleva acabo en una rama independiente.
    - Iniciar permisos de super usuario
          
          $ sudo su - svchad02
          $ cd /home/<user_name>/AW1003001_BigDataCompany/
    
    - En la rama `trunk` actualizar el repositorio.

          $ git checkout trunk
          $ git pull

    - Crear rama. El nombre de la rama lleva el formaro `feature/HU<HU_id>-<HU_name>`. Ejemplo `feature/HU2390426-BANIModificacionDeEjecucionDeCargasHoganCISDB2-13Tablas`


          $ git checkout -b feature/HU<HU_id>-<HU_name>

4. **Generar tablas temporales**

    1. **Verificar subdominio nacional:** Si la ingestion tiene como fuente una base de datos DB2 del subdominio DB2_GENERICO_NACIONAL no es necesario crear bases temporales ya que para este dominio se cuenta con acceso a las tablas fuente. En este caso lo que hay que verificar que es el etl apunta a la fuente y no a las bases de datos temporales.

    1. **Iniciar Servidor:** Se debe iniciar el servidor para alojar las tablas temporales. Segun la documentacion hay una persona encargada de esto, asi que lo que habria que hacer es verificar que este activo dicho servirdor. Si no, solicitar que se inicialice. En la documentacion mencionada estan los pasos para realizar la inicializacion del mismo.

    2. **Creación de tablas:** Para crear una tabla temporal se usa el script *`svchad02@<dev_server_ip>/temporalDatabases/createData.sh`* el cual recibe como parametro un archivo con la medata de la tabla a crear. 
        - **Obtener metadata:** La idea es ejecutar una query a la tabla, en produccion, que se quiere replicar con el objetivo de obtener la descripcion de la misma. Esta query se ejecuta en el **[asistente web](http://asistenteweblz-info-bigdata-asistente.apps.ocpprod.bancolombia.corp/menuOper)** en el item `Ejecutar Eval` del menu `Servicios`. La query va a depender del tipo de base de datos. Por ejemplo para una base de datos *Oracle*.

              "SELECT table_name, DATA_TYPE, COLUMN_NAME, NULLABLE, DATA_PRECISION, DATA_SCALE, DATA_LENGTH FROM  all_tab_columns WHERE table_name='<table_name>' and owner='<esquema>' " 

        3.1 se debe de tener en cuenta que en cada motor cambia el query estos son algunos ejemplos según el motor

        
            • Sybase
            "sp_columns <nombre_tabla>,<esquema>"

            •	Mysql
            "SELECT TABLE_NAME, DATA_TYPE, COLUMN_NAME, IS_NULLABLE, NUMERIC_PRECISION, NUMERIC_SCALE, CHARACTER_MAXIMUM_LENGTH  FROM INFORMATION_SCHEMA.COLUMNS WHERE TABLE_SCHEMA='<equema>' AND TABLE_NAME='<nombre_tabla>'"

            •	Oracle
            "SELECT table_name, DATA_TYPE, COLUMN_NAME, NULLABLE, DATA_PRECISION, DATA_SCALE, DATA_LENGTH FROM  all_tab_columns WHERE table_name='<nombre_tabla>' and owner='<equema>' "

            •	Postgresql
            "SELECT table_name, data_type, column_name, is_nullable, numeric_precision, numeric_scale, character_maximum_length FROM information_schema.columns WHERE table_schema = '<equema>' AND table_name   = '<nombre_tabla>'"

            •	Db2
            "SELECT table_name, data_type, column_name, is_nullable, numeric_precision, numeric_scale,character_maximum_length FROM sysibm.columns WHERE table_schema IN ('<esquema>') and table_name in ('<nombre_tabla>')"

            •	SQLServer
            "sp_columns <nombre_tabla>,<esquema>"

        Para otros tipos de bases de datos verificar el archivo `QUERIES PARA OBTENER DESCRIBE DE LAS TABLAS SEGÚN BASE DE DATOS.doc` en la carpeta de documentacion. El schema, subdominio y el nombre de la tabla debeiran de encontrarse en la Historia de Usuario o en la plantilla de excel descargada. La base de datos temporal se crea en desarrollo. Tener en cuenta que la query que se realiza en el asistente es muy sensible a caracteres espacio antes y despues de la sentencia SQL.

        - **Guardar medatada**: El resultado de la query anterior debe ser guardado en un archivo de texto `.txt` sin dejar ningun espacio o salto de linea al final del archivo. El nombre de este archivo debe llevar la convension `<motor_db>_<table_name>.txt`, por ejemplo para una tabla de oracle con nombre **INQUIRT_AUDIT_TABLE**, el nombre del archivo a creado seria: `oracle_INQUIRT_AUDIT_TABLE.txt`. Este archivo debe ser guardado en el mismo servidor donde se encuentra el script `createData.sh` en un directorio temporal. ejemplo `/home/<user_name>/tablas_temporales/`. Este directorio debe estar en el home del desarrollador.

            **Nota:** En caso de que el nombre de las tablas superen el numero de caracacteres permitidos y tengan un alias, igual nombrar el archivo txt con el nombre de la tabla y el motor. El nombre de la tabla en el archivo no es usado dentro de los scripts, es utilizado unicamente para llevar trasavilidad. El nombre del motor debe ir en minusculas y este si es utilizado durante el proceso.

            Puede que sea necesario crear el directorio `tablas_temporales`.

              $ cd /home/<user_name>/
              $ mkdir tablas_temporales

        - **Crear tabla:** Con el archivo de metadata creado se procede a ejecutar el script `createData.sh`. Este archivo debe de ejecutarse con el usuario administrador.

              $ sudo su - svchad02
              [svchad02/temporalDatabases/]$ sh createData.sh <path_txt_with_metadata>

            Luego de ejecutar el script, se crea la base de datos temporal y se agregan datos random a la tabla. Este script debe de ejecutarse una vez por dia. Ya que al final del dia se eliminan todas las tablas temporales.

            `<path_txt_with_metadata>` ejemplo: oracle_INQUIRT_AUDIT_TABLE. Es obligatorio que el nombre de la tabla este completamente ne minuscula

        El proceso de extraer la metadata y ejecutar el scipt `createData.sh` debe realizarse para cada una de las tablas requeridas. Cuando el desarrollo se extienda mas de un dia, solo la ejecucion del script `createData.sh` debe de repterse.

    3. **Ajustar configs y passwords:** Las tablas creadas hay que ajustarlas con la configuracion y passwords que tienen en produccion. En caso de que el backup ya exista, nos saltamos este paso y nos dirigimos a **4. Modificar Eval**.

        - **Backup archivos:** Copiar los archivos `.config` y `.password` del directorio *`/etl/<subdominio>/`*. Se recomeinda que estos archivos sean copiados en un nuevo directorio  *`/home/svchad02/eval/<subdominio>-backup/`*. Estos archivos contienen la configuracion de las tablas que se tomaran como fuentes para la ingesta. Este backup se hace por trasabilidad por posibles fallos. 

              $ cd /home/svchad02/eval
              $ mkdir <subdominio>-backup
              $ cd <subdominio>-backup
              $ hdfs dfs -get /etl/<subdominio>/<subdominio>.config
              $ hdfs dfs -get /etl/<subdominio>/<subdominio>.password

        - **Agregar config:** Se debe agregar la configuracion de las tablas temporales para que cuando se pruebe la ingestion tomen como fuente las tablas temporales.

              $ cd /home/svchad02/temporalDatabases/templateConfig
              $ hdfs dfs -put template_<motor_db>.config /etl/<subdominio>/<subdominio>.config
              $ hdfs dfs -put template_<motor_db>.password /etl/<subdominio>/<subdominio>.password
                
            Esta sentencia modifica la configuracion del acceso a la base de datos fuente de parte del proceso de ingesta. Lo que estamos haciendo es llevando os datos de conexion de las temporales a los subdominios que consultara la ejecucion de la ingestion.
            
        El proceso de ingestion, copiar de una base de datos a HDFS usando sqoop, se hace executando un script de python. Dicho script se conecta a la base de datos fuente, en este caso una base de datos temporal, y copia los datos. Los cambios de configuracion anteiores modifican el archivo de configuracion que es utilizado por el script de python meniconado, el cual sera generado y ejecutado posteriormente en esta guia. Este proceso se realiza ya que anteriormente las pruebas no se hacian con tablas temporales si no que directamente con las tablas de los usuarios, lo cual presentaba muchos inconvenientes.

    4. **Modificar Eval:** Se debe modificar el archivo que nos ayuda a comprobar de manera rapida la existencia y conexion a las tablas temporales. Hay que tener en cuenta que si en la plantilla existen tablas con distintos prerrequisitos o con distinta periodicidad de ingestion deben crearse distintos paquetes. Para esto ultimo se podrian generar copias distintas de la plantilla donde en cada copia iria la informacion de las tablas correspondinete a cada paquete. Para el autoinventario si podria ir la plantilla con todo la informacion en conjunto.
     
           $ cd /home/svchad02/eval
           $ hdfs dfs -get /etl/<subdominio>/<subdominio>.config <subdominio>.eval

        Al nuevo archivo eval se le debe añadir alguna sentencia *`SQL`* y ejecutarlo.

           $ $EVAL <subdominio>.eval

        **Nota:** Al escribir la sentencia *`SQL`* tener en cuenta que el esquema debe ser el esquema de desarrollo (*`DEV`*) y no de produccion (*`PRD`*). Esta informacion se puede encontrar en el archivo `DIA A DIA DE USO DE BASES DE DATOS TEMPORALES.dox` de la documentacion.
   
5. **Generar y executar paquete:** Una vez creada las tablas temporales se puede proseguir a general el script que se encargara de hacer la ingestion. 
    - **Copiar la plantilla:**  Se debe copiar la plantilla `.xlsm` a dos directorios 
        - *`/home/<username>/AW1003001_BigDataCompany/scripts/sqoopContextPy`*
        - *`/home/<username>/AW1003001_BigDataCompany/manualSteps/others/inventory`*
    - **Ejecutar el script:** Es nesecario ejecutar el script `runSqoopContext.sh` para que genere el script en python que llevara acabo la ingesta en la LZ.
         
          $ sh runSqoopContext.sh -i -p HU<HU_id>.xlsm

        Donde *`-p`* coge datos de la plantilla para columnMapping, *`-m`* toma datos del eval, *`-i`* ejecuta el proceso y generar los archivos de ingestion. 

        El programa va a pedir los sigueintes datos.

          $ ingrese el numero de la HU              : <HU_id>
          $ ingrese el nombre de la app             : <aplicativo>
          $ ingrese el nombre bd en hive            : <db_en_hive>
          $ ingrese el nombre del subdominio        : <subdominio>
          $ requiere columns one by one (y/n)       : <n>
          

        **Notas:**
        - En caso del error *`ERROR - 'NOMTBL''NOMTBL'`* se debe a la plantilla y se recomienda usar una plantilla anterior cambiando los datos de general y metadata por los correspondientes.
        - Si por dado caso hay que apartar el paquete de manera manual, se realiza usando la siguiente query.

              INSERT INTO resultados.Inventario_LZ (package,table_name,semilla,proceso,origen_fuente,data_folder,periodicidad,descripcion,solicita_ingestion,dia_ejecucion,hora_ejecucion,prerrequisito) VALUES ('<paquete>','<table_name_source>','<semilla_en_paquete>','<proceso>','<Motor_db>','<dir_data_raw>','<periodicidad>','<descripcion>','<hu-user>','<dia-ejecucion>','<hora-ejecucion>','<prerrequisito>');

    - **Verificar output:**

          $ cd /home/<username>/AW1003001_BigDataCompany/scripts/sqoopContextPy/output
          $ vi SQOOP<PeriodicidadCarga><paquete#>-getContext.py

        1. Revisar errores por exceso de caracteres y quitar monitoreo si son sucursales o si se especifica en la HU. sucursales = BAM, BANI, BANA. Ejm: 
             
               $ WORKFLOWS=[("SyBase_S_Bam_Productos_COBIS_DBO_CL_ENTE_AGENCIA",False)
          
            El ***False*** hace referencia a que no lleva monitoreo.

        2. Si la tabla es DB2 verificar el archivo `USO DE BASES DE DATOS DB2.docx` en la carpeta de documentacion mencionada anteriormente.

        3. Cambiar el esquema de DEV y UAT por los esquemas de las tablas temporales.
        1. Si el esquema era dbo, cambiar a mayusculas excepto en el campo *"srcSchema"*.
        1. Dentro del paquete, el esquema debe aparecer en mayusculas, excepto en el *"srcSchema"*, amenos de que la base de datos lo amerite.


    - **Mover paquete:** El script recien generado debe ser movido al directorio de *`py_workflows`* y se le deben asignar permisos de ejecucion.

          $ mv SQOOP<PeriodicidadCarga><paquete#>-getContext.py /home/<username>/AW1003001_BigDataCompany/py_workflows
          $ chmod 750 SQOOP<PeriodicidadCarga><paquete#>-getContext.py

    - **Borrar datos:** Se recomienda eliminar los datos en el directorio *`/dara/raw/...`*. y *`/etl/...`*. Esta ruta se puede verificar en el get-context.

          $ hdfs dfs -rm -R <raw_data_location>
          $ hdfs dfs -rm -R /etl/<subdominio>/<table_name_dst>

        Tambien seria buena idea borrar la tabla que se creo de manera temporal 

          $ impala-shell -i $STR_CNX_IMPALA -k -q "TRUNCATE TABLE <bd_hive>.<tablenameLz>;" --ssl
          $ impala-shell -i $STR_CNX_IMPALA -k -q "DROP TABLE <bd_hive>.<tablenameLz>;" --ssl

          # Exmample
           $ impala-shell -i $STR_CNX_IMPALA -k -q "DROP TABLE S_Bam_Productos.teBanking_DBO_DETALLELOTECHEQ;" --ssl


    - **Ejecutar paquete:** El paquete, get-context o script debe ejecutarse para realizar un primer proceso de ingesta. Luego se realizaran pruebas para comprobar que la ingesta se haya realizado correctamente.

          $ python3 submit.py SQOOP<PeriodicidadCarga><paquete#>-getContext.py

          $ nohup python3 submit.py SQOOP<PeriodicidadCarga><paquete#>-getContext.py <Nombre del flujo> &

        Mientras el proceso se esta ejecutando se debe verificar en HUE, en la seccion de jobs, que se este llevando la ingesta. HUE de desarrollo. Una vez la ingesta se haya realziado con satisfaccion continuar con el proceso. De caso contrario volver a ejecutar o verificar los posibles errores.

        Los errrores pueden explorarse con el comando de impala 

          $ yarn logs --applicationId <hue_aplicaction_id>

        `<hue_aplicaction_id>` se peude tomar desde HUE yendo a la tarea que fallo. Revisando el output podria realziarse un diagnostico del error que causop que la ingestion no se llevara acabo. Para ver los logs de un flujo que se ejecuto en PRD debe de usarse el asistente Web. Posiblemente debamos entrar a ver los logs en la tarea map-reduce, para ello buscamos el id del job, cual puede estar en una linea mas o menos como la siguiente 
            
          Running job: job_1670355110358_0227

        En este caso cambiamos la palabra `job` por `application` y con el comando de yarn logs podemos visaluar los logs.
    - **Revisar errores:**
        - Si el error es en **refresh_metadata** o **invalidate_metadata** revisar cuantos datos trajo en el paso de doIngest ya que si no trae ningun dato no se creara la metadata, por ende no se podra hacer un refresh ni un invalidate.     

7. **Automatizacion de certificacion:** Consiste en ejecutar pruebas con el objetivo de verificar que la ingestion se haya llevado de manera correcta.
    - **Crear lista de flujos:** Se debe crear un archivo txt que contenga lo siguiente.
        
          $ <NombreDelFlujoComoEnElSqoop>|SQOOP<PeriodicidadCarga><paquete#>|<p|s> 
        
        *`<p|s>`* depende si es python3 o shell. el nombre del flujo se encuentra en el paquete o get-context.py. En un mismo archivo pueden ir listado varios flujos de distintos paquetes para su ejecucion. La idea es poner todos los flujos de todos los paquetes que se hayan ejecutado para comprobar la calidad de la ingestion.
        
        Este archivo debe ser creado en una ruta especifica.

          $ cd /home/<user_name>/AW1003001_BigDataCompany/scripts/pruebas_certificacion_automatizadas/ingestions
          $ vi HU<HU_id>_workflows_list.txt

        En este nuevo archivo creado es donde se pega el contenido mensionado.

    - **Ejecutar pruebas:**

          $ python3 bigdatacompany_uat.py <workflows_list.txt> <DEV|UAT> <5>

        Dado que estamops haciendo pruebas en desarrollo se usan los parametros de **DEV** y **5** respectivamente. El ultimo numero hace referencia al numero de verificaciones.
        - 1. Verificacion de TABLE_NAME
        - 2. Verificacion de COUNT
        - 3. verificacion de METADATA
        - 4. verificacion de DATA_QUALITY
        - 5. hace todas las verificaciones.
        - 6. Se hicieron pruebas el dia anterior. *Aplica para el release* no aplcia para el uso de tbals temporales

    - **Verificar resultados:** En el directorio *`resultado_automatizaciones`* se pueden verificar los resultados de las pruebas a traves de una serie de logs. 
        - Debemos tener en cuenta que los fallos en METADATA puden estar siendo causado por una falta de CASTEO de los datos. Entrar al log y verificar la informacion para tomar las medidas correctivas.
    
        Si hay errores en **METADATA** y **DATAQUALITY** puede que se deban a que las tablas temporales se crean con datos aleatorios, lo que puede generar incosistencias en los tipos de datos. En este caso la HU debe ser enviada a certifiacion manual. Para ello, debe de igual manera realizarse el punto 8 apartir de este punto y luego comunicarse con alguna de las personas encargadas de certificacion para asignar la HU a dicha persona. Estas personas son asignadas en el planing, consultar a la persona con el rol de Project Owner (PO) por esta informacion.

8. **Cerrar desarrollo:** Puede que las pruebas automaticas generen errores dado que se trabaja con tablas temporales con datos aleatorios, aun asi, debe de cerrarse el desarrollo y asignar la HU a alguna persona con el rol de certificador. Cerrar el desarrollo consiste en grandes rasgos en realizar un pull request a trunk de los archivos generados hasta el momento.

    - **Mover a workflows:** El archivo de workflows_list debe ser movido a un directorio para su posterior uso.
    
          $ mv HU<HU_id>_workflows_list.txt /home/<username>/AW1003001_BigDataCompany/manualSteps/acceptanceWorkflowsLists

    - **Crear archivo .ini:** Se debe crear un archivo *`.ini`* para la ejecucion de la ingestion en PRD. El achivo puede ser un arhicvo  *`auto_exe.ini`* o *`manual_exe.ini`*, esto dependera de que si la ejecucion de las pruebas automaticas (punto 7) son satisfactorias o no. Si las pruebas son aprobadas de manera satisfactoria, se crea un archivo *`auto_exe.ini`*, de lo contrario se crea un archivo *`manual_exe.ini`*.

        - **creacion auto_exe.ini:** Un archivo con nombre *`HU<HUid>_auto_exe.ini`* de ser creado en la direccion *`/home/<username>/AW1003001_BigDataCompany/manualSteps/others/auto_exe/`* con el siguiente contenido.

              [deploy]
              cd $WORKING_PATH/py_workflows && nohup python3 submit.py SQOOP<PeriodicidadCarga><paquete#>-getContext.py &

            En caso de que sean varios paquetes

              [deploy]
              cd $WORKING_PATH/py_workflows && nohup python3 submit.py SQOOPDI2258-getContext.py && nohup python3 submit.py SQOOPDI2259-getContext.py

              [rollback]
              echo "no se ejecuto"


        - **creacion manual_exe.ini:** Un archivo con nombre *`HU<HUid>_manual_exe.ini`* de ser creado en la direccion *`/home/<username>/AW1003001_BigDataCompany/manualSteps/others/manual_exe/`* con el siguiente contenido.

              # Desarrollador: Sebastian De Bedout Mesa

              # En el servidor sbmdeblze001 con el usuario el svchad02, en la carpeta $WORKING_PATH/py_workflows ejecutar el/los siguiente(s) comando(s):
              
              python3 submit.py SQOOP<PeriodicidadCarga><paquete#>-getContext.py &

    - **Push a trunk:** Todos los archivos para la ingestion han sido creados, ahora hay que unir la rama en la que se esta trabajando con la rama principal. 
        
          $ cd /home/<username>/AW1003001_BigDataCompany/
          $ git add ./py_workflows/SQOOP<PeriodicidadCarga><paquete#>-getContext.py ./manualSteps/others/manual_exe/HU<HU_id>_<manual|auto>_exe.ini ./manualSteps/acceptanceWorkflowsLists/HU<HU_id>_workflows_list.txt ./manualSteps/others/inventory/HU<HU_id>.xlsm
          $ git commit -m "Feature HU<HU_id>: <HU_name>"
          $ git push origin feature/HU<HU_id>-<HU_name>

    - **Crear pull-request:** En la pagina de [VSTS](https://grupobancolombia.visualstudio.com/Vicepresidencia%20Servicios%20de%20Tecnolog%C3%ADa/_git/AW1003001_BigDataCompany/pullrequest) (donde se gestionan las HU y los repositorios) en la seccion de `Repos` nos dirigimos a la seccion de `Pull request`. Alli podremos verificar la pull request que acabamos de generar al hacer push. En esta pull-request debemos:
        - Revisar que los commits en la pull-request son los commints que se hicieron en el repositorio.
        - Revisar que los archivos de los commit coinciden con los agregados en el repositorio.
        - verificar el nombre de la rama y del pull-request.
        - Asociar *Item de trabajo* es decir la Historia de Usuario. `HU<HU_id>`.
        - Deshabilitar la opcion de cerrar la HU cuando el pull-request sea aceptado.
        - Crear.
        - Finalmente tomar nota del numero del pull-request que se acaba de crear. `Release-<pullrequest_id>`

9. **Trabajar realese:** Aqui estaremos trabajando y revisando cada uno de los estados del desarollo realizado para poder llevar la ingestion a produccion. Para observar el progreso de cada uno de los estados ingresar [VSTS](https://grupobancolombia.visualstudio.com/Vicepresidencia%20Servicios%20de%20Tecnolog%C3%ADa) a la seccion de `Pipelines` y luego `Releases`. Seleccionamos es repositoio que estamos utilizando (`AW1003001_BigDataCompany`) y buscamos el pull-request que acabamos de crear. 

    Veremos un grafico donde se pueden observar disintos estados, alli se haran pruebas y verificaciones de la ingestion desarrollada. Cada estado o recuadro del grafico representa alguna tarea en el camino a produccion, mientras que dicha tarea se encuentre en ejecucion el recuadro aparecera de color azul, verde si se aprobo y rojo si surgio algun error. De presentarse algun error en alguno de los estados verificar los logs, corregir y realizar el redeploy. Ante cualquier duda consultar con compañeros.

    - **Deploy DEV:** Es el primer estado del `release`, este proceso se ejecuta de manera automatica, lo unico que hay que hacer es darle al boton de `deploy`. En este proceso la ingestion sera despleagada en desarrollo. La ejecucion de este estado puede tomar varios minutos o fallar dependiendo de la ocupacion de los servidores de desarrollo. En el caso de un fallo, dar a `redeploy` sin realizar ningun cambio o correccion.
    
    - **Acceptance Test DEV:** Aca se haran nuevamente pruebas a la ingestion. Son las mismas pruebas que se realizaron en el `literal #7` de este manual. Si las pruebas fallaron en dicho momento, lo mas seguro es que fallen en este paso tambien. Si las pruebas en este paso fallan, la HU debe ser enviada a certificion. En caso de que las pruebas no fallen, el proceso seguira estando  a nuestro cargo.
        
        - **Verificar logs:** Una vez se halla ejecutado el estado, nos dirigimos a la seccion de logs para verificar si las pruebas automatizadas fallaron o no.

        - **Las pruebas fallan:** En este caso la HU deben ser enviadas a alguien con el cargo de certificador, esta persona realizara las pruebas y verificaciones de manera manual, luego, realizara los cambios que sean necesarios para que las pruebas sean aprobadas. Las personas con este rol son anunciadas en cada `Planning`. El paso a certificacion realmente se hace en el estado **Reception HU Equipo**, sin embargo, al fallar las pruebas no se logra pasar a dicho estado y hacer la transferencias de la HU. Para ello tendremos que hacer que el `pull-request` pase al siguiente estado sin realizar pruebas modificando algunas variables del release. 
            - Nos dirigimos a la seccion de `Variables` que se encuentra al lado superior izuqierdo del grafico del `release`, junto a la pestaña de `Pipeline` y debajo del titulo del `release`.
            - Damos click al boton de editar, se despleagara un menu y seleccionamos la opcion `Edit release`, **JAMAS** seleccionar `Edir pipeline` si no se esta con la supervision de un desarrollador experto del equipo.
            - Editar las variables *`nombreCelular`*, *`nombreContacto`*, *`numeroContacto`* y *`opcionPruebas`*. Para el nombre de la celula se recomienda copiar y pegar muy cuidadosamente dicha informacion, la existencia de algun caracter `espacio` extra, genera errores. La variable *`opcionPruebas`* dejarla en 0, de esta manera se ejecutara el estado **Acceptance Test DEV** sin realizar ninguna prueba automatizada, lo que permitira seguir al siguiente estado y enviar el `pull-request` a certificacion.
            - Guardar los cambios.
            - Re-deploy el estado  **Acceptance Test DEV**
                
        - **Las pruebas no fallan:** En este caso el estado aparecera sin error, 
        - debemos de cambiar las variables de nombre, celular y telefono. de resto sera automatica.
        

    - **Reception HU Equipo:** 

         - **Completar artifacts:** Nos dirigimos al `artifacts` y en campo de `Release description` se completa y guarda la siguiente descripcion sin borrar el texto *`Triggered by AW1003001_BigDataCompany trunk...`*

               TITULO: <HU_fullname>
               NECESIDAD: Implementación HU<HU_id> para ingestar información de <servicio_aplicativo>
               SOLUCION: Llevar a la LZ la información de Tabla de la Base de datos <motor_db> de <servicio_aplicativo>
               BENEFICIO: Tener la información disponible en la LZ | <revisar columna finalidad en la pestaña general de la plantilla>
               IMPACTO: No se genera ningún impacto | <revisar la columna impacto en la pestaña general de la plnatilla>

        - **Las pruebas en Acceptance Test DEV no fallaron:**
            
            - **Comentar HU:** Se debe realziar un comentario en la HU diciendo que se pasara por 10x y adjutnar un pantallazo de las pruebas aprobadas en VSTS.
            - **Adelantar documentacion:** Se podria ir adelantando procesos que seran necesarios mas adelante. En este punto pueden crearse la *`matriz de escalamiento`* y la *`wikiIT`* de la ingestion desarrollada. Estos procesos estan mejor descritos en item de **Pre-validacion.** En caso de que se trate de algun tipo de modificacion de ingestion es bueno verificar que la documentacion esta diligenciada correctamente.
            - **Resume:** Se debe dar resume al estado.
    
        - **Las pruebas en Acceptance Test DEV fallaron:** En este caso lo que se debe realizar es preparar la `pull-request` para enviarla a certificacion. **Nota**: No dar en el boton de `Resume`. Se deben realizar las siguientes tareas:
           

            - **Contactar a un certificador:** El proceso de certificacion debe ser llevado acabo por una persona con el rol de certificador, a dicha persona se le debe asignar la `HU`, por ende, se debe hablar con alguna de las personas con este rol para confirmar quien esta disponible para tomar la HU. Las personas con este rol son anunciadas en el `Planing`, en caso de que no se tenga informacion de las personas con este rol, comunicarse con la persona con el rol de PO. Una vez definida la persona que tomara la `HU`, continuar con los siguientes pasos.
            - **Correo promover objetos:** Se debe enviar un correo para notificar al equipo de este cambio de la `HU` desde desarrollo a certificacion. Este correo debe ser escrito de la siguiente manera. **Nota:** En este correo debe ir adjunta la plantilla de la `HU`.

                  ===================== Asunto =====================
                  PROMOVER OBJETOS - De Desarrollo (sbmdeglzm001) a Certificación (sbmdeqlze001)
                  ==================================================

                  
                  ================== Destinatario ==================
                  CelulaLZ
                  ==================================================


                  ===================== Cuerpo =====================
                  ¡Hola equipo, espero que todos es encuentren muy bien!
                  
                  Se adjunta la plantilla respectiva para cumplir con la siguiente HU.

                  HU<Hu_id>: <HU_url>
                  AW1003001_BigDataCompany
                  Release-<release_num>
                  <artifact>

                  De [Desarrollo] a [Certificación] 

                  TITULO: <HU_fullname>
                  NECESIDAD: Implementación HU<HU_id> para ingestar información de <servicio_aplicativo>
                  SOLUCION: Llevar a la LZ la información de Tabla de la Base de datos <motor_db> de <servicio_aplicativo>
                  BENEFICIO: Tener la información disponible en la LZ | <revisar columna finalidad en la pestaña general de la plantilla>
                  IMPACTO: No se genera ningún impacto | <revisar la columna impacto en la pestaña general de la plnatilla>

                  NOTA: -Este despliegue no afecta la disponibilidad del servicio en la malla de operaciones.

                  Objetos a promover:
                  <Pantallazo de los archivos del pull-request>
            
            - **Comentar HU:** Se debe agregar un comentario en la discusion de la `HU` donde se explique a quien se asigna y las razones por las cuales se hacen la nueva asignacion.
            - **Asignar HU:** Finalmente se debe cambiar al asignado de la `HU` por la persona de certificacion con la que se comento acerca del proceso.
            - **Adelantar documentacion:** Mientras que la `HU` se encuentra en certificacion se pueden ir adelantando procesos que seran necesarios mas adelante. En este punto pueden crearse la *`matriz de escalamiento`* y la *`wikiIT`* de la ingestion desarrollada. Estos procesos estan mejor descritos en item de **Pre-validacion.**       
        
    - **Deploy CER:** En caso de haber enviado la ingestion a certificacion. Este estado sera manjeado por la persona certificadora. Em caso contrario este proceso a de pasar de manera automatica, ya que las pruebas fueron aprobadas en el entorno de pruebas.
    - **Manual Step Execution:** En caso de haber enviado la ingestion a certificacion. Este estado sera manjeado por la persona certificadora. En caso contrario en este estado se debe montar el **TestPlan**, el **DoD** y luego si hay que darle **Resume**. Hay guias para la creacion del [test plan](../test_plan/ingestiones.md) y [dod](../dod/README.md)
    - **Acceptance Test UAT:** En caso de haber enviado la ingestion a certificacion. Este estado sera manjeado por la persona certificadora. si vamos por 10x resume.


          Las pruebas para este paso a PRD se realizan automáticamente y las evidencias se encuentran en el stage “Acceptance test DEV” en la etapa Deployucd.

    - **Manual Test:** En caso de haber enviado la ingestion a certificacion. Este estado sera manjeado por la persona certificadora. En caso de qeu vayamos por 10x debemos que  agregar un comentario cuando le damos al resume. El doD ya deberia estar aprobado

        Evidecias VSTS: <Id test plan>


    - **Pre-validacion:** En este estado la documentacion de la ingestion debe estar totalmente diligenciada y se debe realizar una serie de verificaciones antes dar al boton Deploy. En caso de que las pruebas en el estado **Acceptance Test DEV** hayan fallado y la `pull-request` haya terminado su ciclo en certificacion, nos sera retornada en este estado. 

        **Nota:** Hay una guia para realizar generar la *`matriz de escalamiento`* y la *`wikiti`* de manera automaticas. Dicha [guia](https://teams.microsoft.com/_?culture=es-co&country=CO&lm=deeplink&lmsrc=homePageWeb&cmpid=WebSignIn#/docx/viewer/teamsSdk/https:~2F~2Fbancolombia.sharepoint.com~2Fteams~2FCelulasLZ~2FDocumentos%20compartidos~2Ftest~2FCDH%20-%20BigDataCompany~2Fscripts~2FAutomatizacion_Wikiti~2FGu%C3%ADa%20configuraci%C3%B3n%20script%20automatizador%20WIKITI.docx?threadId=19:3d915a44d9e040d59d7946d033299212@thread.skype&subEntityId=%257B%2522web%2522%253A%2522https%253A%252F%252Fbancolombia.sharepoint.com%252Fteams%252FCelulasLZ%2522%252C%2522list%2522%253A%2522https%253A%252F%252Fbancolombia.sharepoint.com%252Fteams%252FCelulasLZ%252FDocumentos%2520compartidos%2522%252C%2522folder%2522%253A%2522%252Fteams%252FCelulasLZ%252FDocumentos%2520compartidos%252Ftest%252FCDH%2520-%2520BigDataCompany%252Fscripts%252FAutomatizacion_Wikiti%2522%257D&fileId=69f3f179-3bad-4d38-83af-5c2b242c952a&ctx=openFilePreview&viewerAction=view) esta en la siguiente direccion de los archivos del equipo **CelulaLZ** de *Teams* *`Desarrollo/CDH – BigDataCompany/scripts/Automatizacion_Wikiti/`*

        - **Determinar el nombre del proceso:** El proceso de ingestion en produccion contara con un nombre del proceso con el cual los usuarios podran hacer monitoreo o reportar algun inconveniente, ya que cada proceso tiene asociado una *`wiki`* y una persona encargada en caso de algun incidente. Incluso hay numero celular al que los usuarios se peuden comunicar ante cualquier fallo. El nombre de paquete tiene el siguiente formato. *`SQP<periodo_id><HU_id><servicio_aplicativo>`*. Ejemplo, una ingestion diaria cuya HU es 2148 y aplicacion es SUELAS, llevara por nombre de paquete *`SQPDI2148SULEAS`.*
            
              <process_name> = SQP<PeriodicidadCarga><paquete#><servicio_aplicativo>

        - **matriz de escalamiento:** El nuevo proceso de ingestion debe registrarse, para ello accedemos a la pagina web de la [matriz de escalamiento](http://vsc.bancolombia.corp/vsti/Lists/Procesos/Ambientes.aspx) y diligenciar el formulario para un nuevo proceso (*nuevo elemento*). El formulario debe ser diligenciado de la siguiente manera:
            |campo|value|
            |-|-|
            |Aplicativo o Plataforma|SQP|
            |Proceso|`<process_name>`|
            |Producto Soportado|Hub de informacion|
            |subdominio|Soporte informacion|
            |Responsable1|Soporte Gioti Primer Nivel|
            |Telefono1 |Grupo Teams|
            |Descripcion|Proceso que ejecuta la extraccion desde SQOOP de las siguientes fuentes asociadas al subdominio de `<subdominio>`: `<flujos>`|
            |Accion|Comunicarse con Soporte y Calidad Gioti ext 44923|
            |Ref Product Stand By|CA Landing Zone|
            |Elemento de configuracion|Sqoop_AW0988001|
            |Prioridad Incidente|Baja (a menos de que se especifique lo contrario)|
            |Ambiente|Produccion|
            |Codigo aplicacion|EVC00037|

            Antes de terminar de diligenciar el proceso, se recomienda guardar en enlace del formulario. Este posiblemente sera utilizado mas adelante. Finalmente guardar en nuevo formulario diligenciado. El registro del proceso puede tardar alrededor de 10min, luego de esto se puede hacer una busqeda por el nombre del proceso en la pagina web de la matriz de escalamiento. Si la busqeda es exitosa guardar el enlace del proceso ya registrado y no la del formulario.

            **Nota:** Tener muy ecuenta la diferencia entre el nombre del paquete y el nombre del proceso. Dado que son cosas distintas hay que diferenciarlas muy bien.

        - **Wiki:** Llenar wiki en azuer ~~**WikiIT:** Con el proceso registrado en la matriz de escalamiento se procede a crear la [wikiIT](http://wikiti/wikiti/index.php/). La WikiIT es aquel lugar donde se puede conocer informacion de cada una de las ingestiones en produccion, al ser nuestra ingestion nueva, debemos agregar toda la informacion necesaria acerca de la misma. Para ello, nos basaremos en un articulo de wiki de otra ingestion.~~
            - ~~Accedemos a la [wikiIT](http://wikiti/wikiti/index.php/) e iniciamos sesion.~~
            -~~ Hacemos una busqueda del proceso **SQPDI1692DIGIWAVE**.~~
            - ~~damos click en *editar codigo*~~
            -~~ Copiamos todo el codigo del text area que aparecio y lo pegamos en algun archivo de texto temporal.~~
            - ~~Volvemos al Home de la wikiIT y realizamos una busqueda del proceso que queremos registrar. Saldra que el proceso no existe y que si queremos crear la pagina, damos click.~~
            - ~~Una nueva pagina se abrira, pegamos el codigo que pegamos en el archivo temporal y damos click en crear pagina.~~
            - ~~Procedemos editar la pagina que acabamos de crear ~~
                - ~~En **DESCRIPCIÓN GENERAL DEL PROCESO** Modificamos los items 1, 2, 7 y 15.~~
                - ~~En **ENTRADAS Y SALIDAS DEL PROCESO** en 1 y 3 ponemos el valor de `<nombre_app>`, en dos dejamos Baja si no se especifica lo contrario en la HU o plantilla. ~~
                -~~ En **INFORMACIÓN TÉCNICA DEL PROCESO - ANEXOS** borramos el enlace que haya en el item 1. ~~
                -~~ En **MANEJO DE INCIDENTES** en el item 1 remplazamos el primer enlace por el enlace del proceso en la `matriz de escalamiento`. ~~
                - ~~En **GESTIÓN DEL CONOCIMIENTO – DOCUMENTACIÓN ERRORES OPERACIÓN (SI APLICA)** modificamos los items 4, 5, 6 y 7 con los datos correspondientes.~~
            - ~~Finalmente guardamos los cambios y el proceso de la WikiIT queda completo.~~
        - **DoD:** En caso de que la `HU` haya pasado por certificacion, este paso estara a cargo de la persona certificadora, cuando certificacion retorne la HU en el estado `Pre-valdiacion`, todo lo referente al `DoD` estara completado.
        - **Test plan** En caso de que la `HU` haya pasado por certificacion, este paso estara a cargo de la persona certificadora, cuando certificacion retorne la HU en el estado `Pre-valdiacion`, todo lo referente al `Test plan` estara completado.
        - **Verificacar datos:** El estado de `pre-validacion` es un estado donde la `HU` esta cerca de pasar a produccion, por ende, hay que hacer una ultima verificacion de los datos de la `HU`, `Release`, `DoD`, `Test plan` y la documentacion:
            - Validación **PMO** titulo **DOD**
            - Validación **HU** titulo **DOD**
            - Validación padre del **DOD**
            - Validación related **DOD**
            - Validación trunk **DOD**
            - Validación Pipeline **DOD**
            - Validación **test plan** numero en **DOD**
            - Validación ruta documentación
            - Validación célula **DoD**
            - Validación sprint **DoD**
            - Validación historia **DOD**
            - Validación Aprobado **DoD**
            - Validación numero de caracteres criterios de aceptación HU
            - Validar que haya una tarea activa
            - Validación **PMO** titulo **test plan**
            - Validación **HU** titulo **test plan**
            - Validar formato titulo **test plan**. *Formato: `<EV>_<Hu_id>`*
            - Validación **Sprint** título **test plan**
            - Valdacion **EVC** en **test plan**. *En el caso del equipo Landing Zone es EVC000337 = EVC - DATOS ANALITICA E IA* 
            - Validacion **Codigo aplicativo** en **test plan**. *En el caso del equipo Landing Zone es NU0044001 o AW1003001.*
            - Validación célula **Test plan**
            - Validación Sprint **Test Plan**
            - Validación descripción **test plan** release
            - Validación descripción **test plan** Título **HU**
            - Validación adjuntos **test plan**
            - Validación padre **test plan** igual que padre **HU**
            - Validación **test plan** con **HU** relacionada
            - Validación **DoD** CERRADO
            - Validación cambio de variables en **release**
            - Validación **release** documentado

            **Nota:** *PMO* hace referencia al primer padre del test plan y del DoD. ~~Que es el mismo codigo de la EVC~~
        
        - ENVIAR CORREO PROMOVER OBVJETOS PDN 

        # TODO Agregar lantilla correo

        - **Deploy:** Una vez todas las verificacion se hayan llevado acabo, hacer click en el boton `Deploy` en el estado del release.
        
    - **Create OC:** Proceso automatico. Es una persona en infra quien esta encargada de este estado. Una vez este aprobado debemos veritficar el DoD, el DoD debe tener la Oc asignado de manera correcta y entrar a uno de los estados del release y ver que si se relaciono con el DoD correcto.
    - **Deploy PDN:** Proceso automatico en el cual como desarrolladores no tenemos mucho por hacer. Hay una persona encargada de este paso. En este estado se envia un correo para promover los objetos a PRD. En caso de que haya pasado por certificacion, este correo lo envia el certificador. si pasa por 10x lo enciamos nosotros. Tambien en este punto el Test plan debe estar cerrado, ya sea que se haya pasado por certi po por 10x.
    - **Execute Auto Inventor:** Proceso automatico en el cual se registra la ingestion en el inventario. La LZ guarda un inventario de todos los procesos que se llevaran a PRD. Este stage se completa de manera automatica, pero puede generar error y no ejecutarse si se pasa una plantilla con datos faltantes.
    
        En caso de que toque registrar la ingestion en el inventario manualmne hay que hacer los sigueintes pasos:

        - **Regsitrar:** Hay que lanzar una query para llenar el inventario. Para esto vamos a solicitar la ayuda a alguien de infra, para que ejecute la sigueinte query
        
              $ impala-shell -i $OOZIE_STR_CNX_IMPALA -k -q "INSERT INTO resultados.Inventario_LZ (package,table_name,semilla,proceso,en_produccion,origen_fuente,subdomain_hdfs,configfile,passwordfile,tbl_lib,hive_database,hive_table_name,short_name,library,etl_folder,data_folder,schema_table_dev,schema_table_uat,schema_table_prod,workflow,subdomine,methods,campo_delta1,campo_delta2,campo_delta3,tipo_de_consulta_fuente,periodicidad,tipo,particionada,descripcion,solicita_ingestion,dia_ejecucion,hora_ejecucion,prerrequisito,en_nube) VALUES ('<paquete>','<table_name_source>','<semilla>','<proceso>','SI','<Motor_DB>','<subdominio>','<subdominio>.config','<subdominio>.password',<hive_db_tablename>,<hive_db>,<tablename-destino>,<aplicativo>,<esquema-fuente>,<ruta-etl>,<riuta-data-raw>,<schema_table_dev>,<schema_table_uat>,<schema_table_prod>,<workflow>,<subdominio>,<tipo-ingestion>,'N/A','N/A','N/A',<tipo_de_consulta_fuente>,<periodicidad>,<Motor_DB>,'Year',<descripcion>,<solicita_ingestion>,<dia_ejecucion>,<hora_ejecucion>,'<prerrequisito>','NO');" --ssl
            
            ejemplo 

            Esta solicutd se hace atra ves de un correo
        
        - **Continar Release:** Hay que pedir ayuda a algun encargado de DevOp para que continue el stage por nosotros.

    - **Execute Manual Commands PDN:** Proceso automatico en el cual como desarrolladores no tenemos mucho por hacer. Hay una persona de infra encargada de estos pasos.
    - **User Validation:** La `HU` retorna a nuestras manos nuevamente en este estado. En este estado se da por finalizada la `Historia de Usuario` dado que ya ha sido llevada a produccion y se da por terminado el `Release`. Para ellos hay que realizar los sigueintes pasos:
        - **Comprobar Ingestion en PRD:** Debemos entrar al aplicativo HUE que se encuentra desplegado en [produccion](https://hue.bancolombia.corp:8889/hue/accounts/login/?next=/). Alli nos dirigimos al Menu de `Jobs` y realizamos una busqueda por el nombre de `flujo` de nuestra ingestion. Accedemos al flujo y verificamos que se haya completado sin ningun error. si usimmos un manual o un auto. Debemos asegurarnos de que el proceso si fue ejecutado ya que esta es la prueba controlada de las ingestiones. Esta prueba es muy importante ya quenos aseguramos de que no habran incidentes que luego puedan impactar al standby. Si la ingestion no estan en HUE-PRD hay que hablar con infra para ver que sucedio.
        - **Consultar tablas PRD:** Hay que hacer consultas alas tablas en PRD recien ingestadas para verificar que no haya ocurrido nada extraño durante la ingestion.
        - **Resume estado Release:** Al haber verificado HUE en produccion nos aseguramos que el proceso se llevo satisfactoriamente y ya podemos validar el ultimo estado del `release`. Damos click al boton de `Resume` del estado **User Validation**. 
        - **Registrar en malla:** Ya que el proceso que desarrollamos se encuentra en produccion hay que registrarlo para que lo pongan en funcionamiento. Este proceso consiste en llenar un Excel para que los encargados de infra sepan que procesos estan pendinetes para iniciar. El registro se hace de la siguiente manera:
            - Vamos a la pestaña **Formato de Ins de Proceso** de la hoja de calculo [FormatoMantenimientoDeProcesosEnProduccionMasivoSQP.xlsx](https://bancolombia.sharepoint.com/:x:/r/teams/CelulasLZ/Documentos%20compartidos/General/Gestion%20Procesos%20Control%20M/FormatoMantenimientoDeProcesosEnProduccionMasivoSQP.xlsx?d=w6e449b241dc24b809176b8b55c01a98c&csf=1&web=1&e=ywPQSl) y en una nueva fila agregamos la informacion de la ingesta que acabamos de desarrollar. Las dos primeras filas son un ejemplo de como llenar dicha informacion. Hay una pequeña guia en la pestaña **Instructivo**.
            - Mencionar a la persona encargada de malla que se realizara el registro de un nuevo proceso. 
            - Una vez la informacion registrada, solicitamos una revision par. Para ello podemos acordar con la persona encargada de certificacion. Los procesos que se agregan al excel que no tienen revision par, no seran inscritas. Las personas que realizan la inscripcion se reunen a las 8:00am, si no hay revision par para ese momento, el proceso no se inscribe.
            - Tener en cuenta que si la ingestion es intraday no se pone max run time y el comando a inscribir es distinto.

        - **Comentar HU:** Hay que comentar la HU indicando que se realizo todo el proceso satisfactoriamente se aconseja verificar una HU cerrada en algun sprint anterior para comprobar la estrutura del comentario. Sin embargo se adjunta un pequeño ejemplo. No olvidar etiquetar a Product owner y al usuario.
        
              @PO @user
              Se realiza el paso a producción y se realizan pruebas controladas en ambiente de producción de forma exitosa. Se inscribe en malla el proceso.

              Flujos: <flujo>
              DBHive: <db_en_hive>
              Aplicativo: <servicio_aplicativo>
              Esquema: <schema>
              Tablas: <lz_table_name>
              Proceso: <process_name>

              <conteo tabla lz>
              <metadata tabla lz>
              <Pantallazo HUE PRD>
              

    - **Cerrar la HU:** La hu debe ser cerrada luego de que se haya inscrito en malla y comprobado que fue ejecutada en control M. Modificar el estado de la HU a `Closed` y guardar cambios. En caso de que sea el ultimo dia del sprint la HU se cerraria luego de pasar por deployPDN pero se debe seguir trabajando. 
    - Verificar el archivo Documetnacion por tipoligia de Hu - cierra en la carpeta de gestion de historias de usuario donde esta la info de que cosas se deben agregar en le cierre de la HU

## Notas del autor: 
- **TODO**: cambiar lastbvalue

echo "0|20220808235959" | hdfs dfs -put -f - /etl/Seguridad_Gestion_Fraude_SQLServer_MINERIA/WEB_REGLAS_FILES_CONSOLI_ENUM/CONSOLI_ENUM_lastValue


- **TODO:** determinar si es onebyone
- **TODO** Esta documentacion debe ser corroborada en la elaboracion de una ingestion.
- **TODO** Verificar ortografia.
- Algunos items de la seccion de preparar datos me queda la duda si sirven para algo o a lo mejor estan desactualizados.
- El proceso de hacer backup a los archivos de config y pasword sigue sin convenserme, para mi es innecesario. Creo que solo tiene sentido para el momento en que se haga un ingestion de tablas temporales por primera vez para una tabla. Si una tabla ya ha sido ingestada con tablas temporales este backup no sirve de nada. Tampoco se lleva una trasabilidad de todos los backups realizados para cada tabla. Cabe aclarar que esto lo menciono desde mi conocimiento del proceso hasta la fecha. 2022-05-16, llevo 3 semanas desde que entre a bancolombia.


        
    


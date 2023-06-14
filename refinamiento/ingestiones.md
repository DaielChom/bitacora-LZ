# Refinamiento Ingestiones.
- **last_update:** 2022-06-15

## Documentacion
- La documentacion del proceso de ingestiones puede ser encontrado en la seccion de [ingestiones](../ingestiones/README.md) de la presetente documentacion.
-	Existe una documentación referente a este proceso en los archivos del equipo **Células LZ** de *Teams* en *`Desarrollo/CDH – BigDataCompany/Bases de datos temporales/`* o en *`test/CDH – BigDataCompany/Bases de datos temporales/`*
-	Hay una serie de videos donde se da capacitación sobre ingestión de tablas temporales, revisar siempre el mas reciente. Dichos videos están en en los archivos del equipo **Células LZ** de *Teams* en *`General/Gestion Historias Usuario/Ingestiones/`*
- Hay un archivo con la info de los refinamientos. [DOR](https://bancolombia.sharepoint.com.mcas.ms/:p:/r/teams/CelulasLZ/_layouts/15/Doc.aspx?sourcedoc=%7BABE3BEB1-E297-43BD-A064-954164546B64%7D&file=DOR-DOD%20DAIA13-14-15-16.pptx&wdOrigin=TEAMS-WEB.p2p.bim&action=edit&mobileredirect=true)

## Checklist
- Leer la historia de usuario.
- Descargar la plantilla.
- Identificar datos.
- Verificar permisos.
- Documentar HU.

## Proceso
En los refinamientos de `Historias de Usuario` de ingestiones, el objetivo es rectificar que los requistos de informacion y acceso para una ingestion esten disponibles para el ingeniero o desarrollador quer la llevara a cabo. En otras palabras, comprobar la existencia y permisos de acceso a tablas en produccion a traves de queries a las bases de datos fuentes que se menciona en la `HU`. El proceso de refinamiento se lleva a cabo de la siguiente manera:

- **Leer la historia de usuario:** Se debe leer la HU y los distintos comentarios que haya para obtener el contexto de la misma. Debemos verificar que informacion nos da el usuario respecto a las tablas que se queiren ingestar. Debemos asegurarnos que la HU cuente con un padre.

- **Verificar HA de permisos:** Debemos asegurarnos que la HA de permisos se encuentra cerrada, de lo contrario no podremos trabajar la ingestion ya que no contamos con acceso a las tabals o bases de datos de la ingestion.

- **Descargar la plantilla:** Las `HU` de ingestiones por lo general traen adjunto un archivo excel con el nombre de plantilla. En este archivo esta toda la informacion del proceso de ingesta que realiza. Por ende, hay que verificar la calidad e integridad de la plantilla. En especifico se deben verificar las pestañas *Info General Fuente* y *Metadata Tecnica Tabla DB*.
    
    - **Info General Fuente:** Esta pestaña debe contar con informacion en los campos:
        - nombre de la tabla
        - servicio aplicativo
        - Tipo BD
        - Tipo de carga
        - Periodicidad
        - Prerrequisito
        - Hora de prerrequisito
        - Nombre de la columna incremental.
        - Base de datos en Hive.
        
        La columna **prerrequisito** puede estar vacio si las tablas a ingestar no requieren prerequisito. La columna **incremental** es obligatria si se trata de una ingestion con tipo de carga `Incremental.` La cantidad de elementos en la columan **Nombre tabla** debe de coincidir con el numero de tablas a ingestar indicada por el usuario en la `HU`. Tambien hay que tener en cuenta que el **nombre de la tabla** cuente con el tamaño requerido. Cuando no hay **prerrequisito** es necesario el valor de **Hora de prerrequisito**, en caso contrario la ingestion se hace luego de la ingestion del prerrequisito.

        - **Tamaño nombre de tabla:** Los nombres de cada tabla deben contar con una cantidad decaracteres menor a 15. En caso de que el nombre de la tabla sea superior a 15, el usuario debe proposionar un alias que no supere dicho limite. Esto se debe a que la ingesta se creara con un nombre que tiene el formato de *`<bd_hive>_<nombre_aplicacion>_<nombre_schema>_<table_name>`*. Este nombre del flujo de ingestion no puede superar los 32 caracteres. Una forma de corroborar el tamaño del alias de la tabla es usando el excel [VALIDACION-HU-PLANTILLAS](https://bancolombia-my.sharepoint.com/:x:/r/personal/julansan_bancolombia_com_co/Documents/Microsoft%20Teams%20Chat%20Files/VALIDACION-HU-PLANTILLAS.xlsx?d=wbd31bc853e074fae954850db84848fa5&csf=1&web=1&e=Q4uyJe). Nos dirigimos a la pestaña **TaxonomiaTabla** y alli agregando los campos correspondientes a **APLICATIVO**, **ESQUEMA PRD** y **TABLA o ALIAS** podemos verificar si el nombre de la tabla es correcto o no. El campo del tamaño de los nombre de la tabla y el alias son de gran importnacia, la plantilla debe incluir estos dos campos, en caso de que se realice la ingestion sin estos dos campos se genrara un error en el stage de AutoInventory del release de ingestiones.

            **Nota:** En caso de que la Taxonomia de la tabla no supere la prueba a de indicarsele al usuario, no debemos modificar nada. En tal caso etiqeutar la HU como No se puede trabajar e indicarle al usuario.

    - **Metadata Tecnica Tabla DB:** Esta pestaña debe contar con informacion en los campos:
        - nombre del campo
        - esquema en PRD
        - tipo de dato
        - longitud
        - Precision
        - nulable
        - Formato fecha
        - Nombre tabla.
        
        La columna **formato fecha** es requerido si el campo es de tipo **fecha**. No es obligacion que hagan entrega correcta del subdominio ya que esta informacion pertenece a los equipos de la **Landing Zone**. Para completar esta informacion se expondra mas adelante como determinar el dominio para un conjunto de tablas. La cantidad de elementos en la columan **Nombre tabla** debe de coincidir con el numero de tablas a ingestar indicada por el usuario en la `HU`.

    En caso de que la plantilla no tenga campos requeridos o no este bien diligenciada, nosotros no debemos arreglarla no completar la informacion, ni modificarla. Debemos solicitar al usuario para que la diligencie correctamente y la suba nuevamente a la HU.

- **Identificar datos:** necesitamos los sigueitnes datos de la plantilla dado que realizaremos un comentario en la `HU` para notificar que se puede trabajar.
    - Subdominio.
    - Tipo de Motor.
    - Numero de tablas a ingestar.
    - Esquema en PRD.
    - Tipo de ingestion.

    Como se menciono, el campo del subdominio en la plantilla no es fiable, por ende hay que realizar una busqueda del mismo.

    - **Determinar subdomino:** 
        - Puede estar como comentario en la HU.
        - Puede estar en la Plantilla si el campo no esta con el valor `OTROS`. En este caso verificar con alguna query si el subdominio es correcto.
        - Se puede encontrar en el paquete al cual pertencen las tablas a ingestar. Si las tablas ya han sido ingestadas.
        - El paquete puede estar escrito en los comentarios de la HU.
        - El paquete puede ser identificado realizando filtros del nombre de la tabla o alguna palabra clave en el directorio de los paquetes *`AW1003001_BigDataCompany/py_workflows/`*.
            
              $ grep -irn <key-word>
        
        - Puede ser determinado buscando el subdominio de otros paquetes que sean del mismo aplicativo. Para ello utilizar el comando anterior usando como palabra clave el aplicativo. Luego probar realizar queries con las distintas opciones y verificar la que arroje buenos resultados.
        - Se puede determinar usando el excel [Usuarios de conexion produccion](https://bancolombia-my.sharepoint.com/:x:/r/personal/lualrami_bancolombia_com_co/Documents/Archivos%20de%20chat%20de%20Microsoft%20Teams/Usuarios%20de%20conexi%C3%B3n%20producci%C3%B3n.xlsx?d=wfd9998090df3412fb9cf142bb8c49e38&csf=1&web=1&e=RMaqZN) realizando una busqueda de la base de datos en Ambiente Produccion (`<db_prd>`) que se encuentra en la pestaña `Historia de Usuario`. Verificar que el usuario en la pestaña `Historia de Usuario` concuerda con el usario en el excel `Usuarios de conexion produccion`.
        - Se puede determinar el subdominio realizando una query con el usuario de produccion (`<user_prd>`) que se encuentra en la pestaña `Historia de Usuario`, en el [asistente web](http://asistenteweblz-info-bigdata-asistente.apps.ocpprod.bancolombia.corp/). Nos dirigimos a `Servicios` y luego a `Impala Shell` y ejecutamos la siguiente query.
            
              $ SELECT * FROM resultados.reporte_usuarios_conexion WHERE usuario_conexion LIKE '%<user_prd>%'

- **Verificar permisos:** Para ello realizaremos pruebas de conexion a traves de queries en el [asistente web](http://asistenteweblz-info-bigdata-asistente.apps.ocpprod.bancolombia.corp/). nos dirigimos a `Sevicios` y luego a `ejecutar Eval` y alli realizamos queries de conteo y metadata a cada una de las tablas descritas en la planitlla. El subdominio se utiliza en el campo de `archivo.config` y `archivo.password`

      $ "SELECT COUNT(*) FROM <schema>.<tablename>"
      $ "SELECT table_name, data_type, column_name, is_nullable, numeric_precision, numeric_scale, character_maximum_length FROM information_schema.columns WHERE table_schema = '<esquema>' AND table_name   = '<nombre_tabla>'" 

    Se debe tomar pantallazo del output de los conteos y pegarlos en el cometnario que se haga en la HU y se deben adjuntar archivos txt con el output de la query de metadata, estos archivos deben estar nombrados de la siguiente manera `<motor_db>_<table_name>.txt`. El nombre del motor de la base de datos debe ser en minusculas. Adjuntar la metadata ahorrara tiempo cuando se este desarrollando la HU.

    **Nota:**
    - Es importante que la query tenga las doble comillas.
    - La query de obtener la metadata puede cambiar dependiendo del motor de base de datos. Las queries para cada una de las bases de datos se encuentran en [QUERIES PARA OBTENER DESCRIBE DE LAS TABLAS SEGÚN BASE DE DATOS.doc](https://bancolombia.sharepoint.com/:w:/r/teams/CelulasLZ/Documentos%20compartidos/test/CDH%20-%20BigDataCompany/Bases%20de%20datos%20temporales/QUERIES%20PARA%20OBTENER%20DESCRIBE%20DE%20LAS%20TABLAS%20SEGU%CC%81N%20BASE%20DE%20DATOS.docx?d=wf5e9851e98d443c199443ba45b4c2013&csf=1&web=1&e=AaopXh).
    - Si la query no funciona puede ser que el subdominio o esquema sean los equivacados o puede que haya un espacio al inicio o final de la query, antes o despues de las doble comilla.
- **Existencia:** revisar que la tabla ya no se este ingestando. Buscar nombre, esquema, aplicativo, fuente, etc. La idea es que dos flujos no queden con el mismo nombre, ya que generaria problemas.
- **Documentar HU:** En la HU hay que indicar si se puede trabajar agregando la etiqueta ***Se puede trabajar*** o ***No se puede trabajar*** segun sea el caso. De no poderse trabajar hay que indicar las razones en un comentario etiquetando al usuario y al PO, tambien se deberia contactar al usuario para que arregle la HU y poderla trabajar en el siguiente sprint. En caso de que se pueda trabajar, hay que agregar un comentario en donde se etiquete al usuario y al PO donde se muestren a traves de pantallazos el resultado de conteo de las tablas a ingestar y se debe de adjuntar los txt con la metadata de cada tabla. Tambien que agregar el Subdominio, Tipo de Motor, Numero de tablas a ingestar, Esquema en PRD y Tipo de ingestion.

    
    


# TO-DO
- Arreglar las personas a quienes hay que ajuntar en los comentarios.
- Arreglar documentacion de lo del padre.

REVISAR SI LA TABLA YA SE INGESTA
- misma taxonomia, aplicativo, esquema, nombre, motor
- buscar en un get context
- verificar el string de contexion. (excel ese de luis)
- en el string deconexion tambien esta la ip
- verificar en control M


VERIFICAR QUE EL CAMPO INCREMENTAL NO SE NULABLE => si es nulable contar los nulos o alguna evidencia que es nulable pero no habranunca nulos.







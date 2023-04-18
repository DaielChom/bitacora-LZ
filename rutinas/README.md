# Rutinas virtuales
- **state:** Building
- **last_update:** 2022-06-07

Consiste en el proceso de crear o modificar alguna libreria o codigo (rutina) del repositorio principal de banco (artifact). Para estas modificaciones se nos entrega un pull-request donde se evidencian los cambios requeridos, el equipo de **Landing Zone**  se encarga de revisar y aprovar dichos cambios. En otras palabras es codigo de usuarios que se ejecuta en nuestros servidores. Esta revision y aprovacion se realiza usando una serie de script validadores y pruebas controladas utilizando un repositorio propio de la LZ. Existen dos tipos de HU para rutinas, Modificaciones o nuevas rutinas. 

## Documentación
- https://bancolombia.sharepoint.com/teams/CelulasLZ/Documentos%20compartidos/Forms/AllItems.aspx?csf=1&web=1&e=7ucczj&xsdata=MDV8MDF8fDhlNDk5YjAyYTVlMjQyZmI3NDBhMDhkYWE3MWJlMGM0fGI1ZTI0NGJkYzQ5MjQ5NWI4YjEwNjFiZmQ0NTNlNDIzfDF8MHw2MzgwMDYwMzU0MDMyNzMyMjZ8R29vZHxWR1ZoYlhOVFpXTjFjbWwwZVZObGNuWnBZMlY4ZXlKV0lqb2lNQzR3TGpBd01EQWlMQ0pRSWpvaVYybHVNeklpTENKQlRpSTZJazkwYUdWeUlpd2lWMVFpT2pFeGZRPT18MXxNVGs2YldWbGRHbHVaMTlPVkdNeFdsZE9hazlFU1hST1JFcG9Ubmt3TUU1VVRYZE1WR2QzVGxkSmRGbHRXVFJhVkdjeFRVUmpNVTlIVG0xQWRHaHlaV0ZrTG5ZeXx8&sdata=VnFXNTNBNzVOV2VMUEdsMnplK1pEU3dIbW5zVC9xN2d0M3MveVRzMUpaYz0%3D&cid=2f349da6%2Db808%2D44f1%2D95b5%2D26438cfb3087&RootFolder=%2Fteams%2FCelulasLZ%2FDocumentos%20compartidos%2Ftest%2FGestion%20Conocimiento%2FRutinas%20An%C3%A1liticas%2FNuevo%20Modelo%20Rutinas&FolderCTID=0x01200088640B173FD09044880D25110D2565F4
- Hay una serie de videos donde se da capacitación sobre rutinas virtuales y una serie de documentos sobre rutinas en los archivos del equipo **Células LZ** de *Teams* en *`General/Gestion Historias Usuario/Rutinas/`*
- Como primera aproximacion a rutinas se aconseja ver los siguientes videos en el orden correspondiente: [Primer video](https://teams.microsoft.com/_?culture=es-co&country=CO&lm=deeplink&lmsrc=homePageWeb&cmpid=WebSignIn#/mp4/viewer/p2p_ns/https:~2F~2Fbancolombia-my.sharepoint.com~2Fpersonal~2Fefcabal_bancolombia_com_co~2FDocuments~2FArchivos%20de%20chat%20de%20Microsoft%20Teams~2FCapacitaci%C3%B3n%20rutinas%20Ambientes%20virtuales-20220112_100814-Grabaci%C3%B3n%20de%20la%20reuni%C3%B3n.mp4?threadId=19:0e4f6bd01ec545a2929d802004e36a7d@thread.v2&fileId=05851122-32fb-4e17-96b7-7abd9e663fe2&ctx=bim&viewerAction=view), [Segundo video](https://teams.microsoft.com/_?lm=deeplink&lmsrc=homePageWeb&cmpid=WebSignIn#/mp4/viewer/teamsSdk/https:~2F~2Fbancolombia.sharepoint.com~2Fteams~2FCelulasLZ~2FDocumentos%20compartidos~2FGeneral~2FGestion%20Historias%20Usuario~2FCapacitaciones%20Dllo~2FCapacitacion%20Rutinas%20Ambientes%20Virtuales.mp4?threadId=19:553615d04a5b4936bfc68e6d2bd319b2@thread.skype&subEntityId=%257B%2522web%2522%253A%2522https%253A%252F%252Fbancolombia.sharepoint.com%252Fteams%252FCelulasLZ%2522%252C%2522list%2522%253A%2522https%253A%252F%252Fbancolombia.sharepoint.com%252Fteams%252FCelulasLZ%252FDocumentos%2520compartidos%2522%252C%2522folder%2522%253A%2522%252Fteams%252FCelulasLZ%252FDocumentos%2520compartidos%252FGeneral%252FGestion%2520Historias%2520Usuario%252FCapacitaciones%2520Dllo%2522%257D&fileId=3caa8f02-9da9-43b1-909c-b35faf213ac6&ctx=openFilePreview&viewerAction=view), [Tercer video](https://teams.microsoft.com/_?culture=es-co&country=CO&lm=deeplink&lmsrc=homePageWeb&cmpid=WebSignIn#/mp4/viewer/p2p_ns/https:~2F~2Fbancolombia-my.sharepoint.com~2Fpersonal~2Flualrami_bancolombia_com_co~2FDocuments~2FGrabaciones~2FSegunda%2520Parte%2520Capacitaci%25C3%25B3n%2520Rutinas-20220505_150913-Grabaci%25C3%25B3n%2520de%2520la%2520reuni%25C3%25B3n.mp4?threadId=19:0e4f6bd01ec545a2929d802004e36a7d@thread.v2&fileId=E72CF5C9-CEDC-4438-A7DF-1B3560298D37&ctx=bim&viewerAction=view)

## Proceso.
0. **Crear tareas:** **TO-DO**
1. **Revisar HU:** Para iniciar con el desarrollo de la HU hay ciertas verificaciones que se aconsejan realziar:
    - Verificar que la `HU` es trabajable.
    - Leer los comentarios de la `HU` para tomar el contexto.
    - Revisar logs y que cumplan con los requisitos solicitados:
        - Una minima de archivos de 10 logs.
        - Archivos de logs para distintos dias, minimo para 2.
        - Una separacion entre archivos de logs de minimo 2 horas.
        - Que el orquestador haya finalizado satisfactoriamente.
        - Que no haya errores en los logs.
    - Revisar el `pull-request` asociado a la rutina.
    - Revisar el repositorio de la rutina
        - Revisar el archivo `config.json` para determinar la zona de resultados.
    - Si la HU es de modificacion verificar que ya esta en produccion.
                
    **Notas:**     
    - En el proceso de refinamiento se llevan a cabo las revisiones necesarias para trabajar la `HU`, sin embargo una segunda revision rapida no esta de más. 
    - Los requisitos de archivos de log, estan siendo revisados.
    - Hay ciertos **ERRORES** en logs que se dejan pasar, como lo son intermitencas con Impala.
    - Para mayor informacion de los pasos de revision, ver las instrucciones del refinamiento de `Historias de Usuario` de `rutinas`.    

2. **Preparar datos:** Se requieren distintos parametros, variables o informacion en varias partes del proceso. Es bueno tener esta serie de datos desde el primer momento del desarrollo de la HU.
       
    |Parametro|Identificador|Fuente|Ejemplo|
    |-|-|-|-|
    |Numero HU|`<HU_id>`|Historia de usuario|3224139
    |Nombre HU|`<HU_name>`|Historia de usuario|(Daniel Andres Varon Quesada) Circular 32 - Ingestion Fuente conocida SULEAS LSLOCARRBF - 1 tabla|
    |Artifact|`<artifact>`|En el PR adjunto a la HU. Despues de aprobar el PR|20220111.1
    |Codigo PMO|`<codigo_PMO>`|El primer padre de la HU|EVC00037|
    |Nombre Celula|`<celula_name>`|Historia de usuario|DAIA15 - DRACARYS|
    |Nombre repositorio|`<repository_name>`|En el PR adjunto a la HU|bana-vrie-calendarizacion-fco2|
    |Nombre del paquete|`<package_name>`|Nombre en el setup.py|alm_bana_fco2
    |site'package|`<site-package>`|Nombre de la carpeta dentro de `src` en el PR adjunto a la HU|alm_bana_fco2
    |Zona de Resultados|`<results_zone>`|En el config.json de la PR. En los archivos PY de la PR. Preguntar al Usuario.|proceso_bana_vp_riesgos
    |Periodicidad|`<periodicidad>`|Por lo general se encuentra en la HU|Diaria
    |autoInventory|`<auto_inventory>`| true o false si se trata de una calendarizacion o una modificacion respectivamente.

    **Nota:** 
    - Como buena practica en los usuarios, el nombre del repositorio y el nombre del paquete, deberian ser iguales solo que el primero separado por `-` y el segundo por `_`. **Esto no afecta en nuestro proceso de Rutinas.**
    - La periodicidad es para rutinas de calendarizacion o modificaciones en la periodicidad. Este dato se usar a la hora de inscribir en malla.
    - **autoInventory:** Es una variable que debe ser diligenciada en el relese, sirve para realizar el insert en el autoinventario de la LZ, una tabla que contiene todos los procesos que salen a PRD en la LZ. Para las rutinas debe de realizarse en insert cada vez que se despliega por primera vez los archivos en PRD.

3. **Nuevo release:** El objetivo de la rutina es verificar, probar y productizar cambios en alguna libreria de los clientes. Para ello el equipo Landing Zone cuenta con un repositorio en donde estan los scripts y entornos virtuales con los cuales realizar estas tareas. Para llevar seguimiento a todo este proceso de la rutina debe crearse un release con el cual estaremos trabajando a partir de este momento. Pare ello se debe:
    - **Aprobar PR:** Aprobar la `pull-request` adjuntada por el usuario en la `HU`, luego de haber hecho las revisiones pertienentes. La idea de estas revisiones es evitar que el usuario tenga que crear un nuevo release luego de habe aprobado alguno con malas practicas.
      - El dsn es `impala-virtual-prd` en el config.json ubicado en la carpeta static.
      - El PR va de master a trunk.
      - Verificar el manifest
      - preguntar por la zona de resultados
      - Verificar que no estuviese aprobado con anterioridad.
      - Que no haya usuarios o rutas quemadas.
      
      Si se encuentran hallazgos con estas revision indicar al usuario que las corrija, luego de tenerlas corregidas se procederia a aprobar el pr. En lo ideal estas revisiones deberian hacerse antes del primer lunes despues del inicio del sprint.

      - Una vez aprobado el pr fijarnos que aparecemos como aprobadores en el Pr.
    - **Contactar usuario:** Indicarle al usuario que continue con el *tag de version* y el *pipeline de despliegue en Artifactory*. Compartir `HU`. Si el usuario no conoce este procedimiento, compartir la siguiente [documentacion](https://grupobancolombia.visualstudio.com/Vicepresidencia%20de%20Innovaci%C3%B3n%20y%20Transformaci%C3%B3n%20Digital/_wiki/wikis/Vicepresidencia-de-Innovaci%C3%B3n-y-Transformaci%C3%B3n-Digital.wiki/9850/Calendarizaci%C3%B3n-de-paquetes-usando-Artifactory). Cuando el usuario haya comentado que se hizo el despligue podemos comprobar busnco en [artifactoty](https://artifactory.apps.bancolombia.com/ui/packages) usando el nombre del paqute.
    - **Validar artefacto:** Una vez el usuario haya terminado las respectivas tareas luego de Approvar el `pull-request`, debemos verificar que el despliegue quedo realziado de manera correcta. Debemos revisar que haya un archivo de version y tambien que el despliegue se haya realizado en la rama **trunk**. El despliegue debe tener un identificador que sera nuestro artefacto, el cual lleva un formato de <curent-day>.<consecutiv> ejempllo: `20221124.2`.
    - **Nuevo release:** Una vez el usuario haya terminado las respectivas tareas luego de Approvar el `pull-request`, nos dirigimos a la seccion de `Pipelines` del portal de [VSTS](https://grupobancolombia.visualstudio.com/Vicepresidencia%20Servicios%20de%20Tecnolog%C3%ADa/_release?_a=releases&view=mine&definitionId=5944) y luego a `Releases`. Alli seleccionamos el repositorio *`AW1003001_CDH_PyEnvs`* y creamos un nuevo `release`. En el formulario de creacion del release poner el `<artifact>` que consultamos anteriormente. Finalmente dar click al boton crear del formulario.
    - **Modificar variables:** Una vez creado el release, ingresamos al mismo y nos dirigimos a la seccion de variables en la esquina superior izquierda, debajo del titulo del release. Alli damos click en editar y modificamos las sigueintes variables:

        |variable|valor|
        |-|-|
        |codigoPMO|`<codigo_PMO>`|
        |Nombre Celula|`<celula_name>`|
        |nombreContacto|`<nombre_desarrollador>`|
        |numeroContacto|`<celular_desarrollador>`|
        |repositoryName|`<repository_name>`|
        |packageName|`<package_name>`|
        |zona|`<results_zone>`|
        |autoInventory|`<auto_inventory>`|

        Luego de modificar las variables, procedemos a guardar. Estas modificaciones de variables se hacen para un correcto procesamiento en el stage de CreateOC del release.

    - Aregar descripcion en el artifact

            TITULO: <HU-fullname>
            NECESIDAD: HU<huid>: <HU description>.
            SOLUCION: <HU description>.
            BENEFICIO: <HU description>.
            IMPACTO: <HU description>.
          
        En caso de que en la HU no se encuentren los datos de necesitdad, solucion, beneficio o impacto, debe de hablarse con la PO. Tambien suelen entregar un word en cada rutina donde s epodria encontrar esa info.

4. **Release: Deploy DEV.** Con el release creado iniciamos la serie de tareas para llevarlo a produccion. La primera tarea consiste en generar el entorno vitual de Python para poder ejecutar la rutina en desarrollo y poderla validar. 
    - **Generar ambiente virtual:** Comenzamos dando click a `Deploy` del primer estado del release **Deploy DEV**. En caso de que se quede encolado se podria hablar con el compañero encargado de la otra pull-request para gestionar el despliegue. Este proceso se llevara acabo de manera automatica. Al finalizar verificar los logs del despliegue automatico. 
    - **Verificar ambiente:** Se hara una verificacion manual de la creacion del entorno virtual. Para ello accedemos al servidor de desarrollo e iniciamos permisos de superusuario con `svchad02`.

          $ sudo su - svchad02
          $ cd /avirtual/<results_zone>/

        Luego de acceder a la carpeta de zona de resultados verificar la fecha de creacion de los directorios. Deben ser directorios creados recientemente.

          $ ls -lstr

        Tambien debemos veritificar si hay una carpeta con el nombre del paquete. Luego se procede a verificar la inslacion de la rutina del usuario.
               
          $ cd <repository_name ó package_name>
        
        Dentro de este directorio debe haber una carpeta `venv` en donde se instalaron todos los paquetes del ambiente virtual requeridos por la rutina. Y un archivo `version.txt` el cual debemos comprobar que tenga la misma version de rutina que la reportada en el artifact (VSTS) en el modulo published. Si no son iguales decirle al usuario que el artefacto no tiene la versión actualizada de la rutina. El release va a descargar e instalar la version que encuentre en el [artifactoty](https://artifactory.apps.bancolombia.com/ui/packages).

        Finalmente vamos a la carpeta de la rutina instalada y la comparamos con la rutina en la `PR`.

          $ cd venv/lib/python3.5/site-packages/<site-package-name>/

        **Nota:** El nombre del paquete (`<package_name>`) y el nombre del sitepackage (`<site-package-name>`) son muy similares, pero pueden llegar a tener diferencias. Hay que tomar nota de ambos, ya que mas adelante se hara uso de ellos.    

    - **Cambiar dsn** una vez en el site package entramois a `static/config` y modificamos el archivo `config.json`. la idea es cambiar el valor de la variable dsn de **impala-virtual-prd** a **impala-virtual-dev**.

5. **Release: Dev Validation.** Nuestra tarea aca sera comprobar el buen funcionamiento de la rutina en el ambiente de desarrollo. En caso de encontrar algun error (Hallazgo) de la rutina, se debe notificar al usuario. La idea es notificar al usuario de la mayor cantidad de hallazgos posibles, si es posible cambiar algo en la rutina para ello hacerlo. Sin embargo, ninguno de los cambios que hagamos debe ser llevado a produccion, nuetra tarea es validar el funcionamiento de la rutina encontrado hallazgos, no corregir codigo. Para el proceso de validacion de la rutina se debe:
    
    - **Validar buenas practicas:** 
      - opcion1: Ejecutar el stage y verificar los logs.
      - opcion2: Existe un script que verifica buenas practicas de `SQL` y Python en la rutina.        
        
            $ cd /home/<user_name>/AW1003001_BigDataCompany/scripts/routineValidator
            $ python routineValidator.py -p "configs/routineValidator.properties" -r "/avirtual/<results_zone>/<repository_name o paquete>/venv/lib/python3.5/site-packages/<site-package-name>/" -n <package_name>

        El -p es el archivo donde estan las propiedades que va a validar. La ejeccion del validador puede dar un error de que el archivo de propiedades no existe o esta desactualizado para ello ejecutamos 

          $ mkdir logs
          $ mkdir hallazgos
          $ cd configd
          $ sh genConfig.sh -g

      - **Revisar hallazgos:**
          cd hallazgos 
          check txt
              si hay allazgos
              si no hay hallazgos
          comentar HU

    - **Ejecutar rutina:** El siguiente paso consiste en ejecutar la rutina y verificar que no haya errores en python o en la ejecucion del codigo. Para ello vamos a usar un script el cual se encarga de realizar dicha ejecucion.

          $ python3 /home/<user_name>/AW1003001_BigDataCompany/scripts/pyEnvs/runPyEnv.py --resultDir <results_zone> --pyEnvPkg <package_name> --pySitePck <site-package-name>

        La rutina puede vallar y cada error debes informarle al usuario para que haga los respectivos cambios. El equipo no es dueño de la libreria orquestador ni de ninguna de las usadas en la rutina, asi que no es nuestro deber ir a profundidad de los errores que se generen para darle explicaciones al usuario, ellos tiene documentacion de cada una de las librerias que usan.
        
       
        La rutina puede fallar por que en dev no tenemos acceso a todas las tablas, es un error esperado, si salen otros errores hay que reportarlos como hallazgos. Pueden salir errores de sintaxis de python u otros errores, cada error es un hallazgos. Algunos errores se pueden dar dado que los ambientes virtuaels se crean con python3.5, los usuarios pueden entregar rutinas con versiones superiores.

        la version de python u otras librerias instaladas se pueden ver en .....

        un ejemplo pueden ser los fstrings xxx en caso de este error se puede agregar xxxx -> solicitarle al usuario

        la idea de ejecutar la rutina es encontrar erores o problemas que se denominana hallazgos, los cuales se reportan al usuario para que los corriga.
        la idea no es botar la pelota con el usuario con cada error que se encuentra, uno mismo podria vcorregir el error para verificar nuevos hallazgos y asi entregar una lista de hallazgos a los usuarios. Se notifica a los usuarios que hagan los cambios ya que los cambios en lasrutinas deben ser por parte de los mismos usuarios, en landingzone no debemos cgenerar ningun tipo de cambio en la rutina que vaya a produccion.

        Nuestra tarea es corroborar al codigo no correguir. Si hay hallazgos y no da tiempo de vovler a probar. la HU no se calendariza, es decir no va a producicon, es decir no se inscribe en mal
        - Fallos: Si falla por algun import puede que el usuario requiera conocer el [repositorio guia](https://dev.azure.com.mcas.ms/GrupoBancolombia/Vicepresidencia%20de%20Innovaci%C3%B3n%20y%20Transformaci%C3%B3n%20Digital/_git/vitd-ejemplo-orquestador2)


    - **Resume:** Cuando nos aseguramos de que la rutina no tiene hallazgos reejecutamos el stage deploy dev y luego le damos resume al estado Dev validation. Esto sucede asi dado que modificacmos el config.json lo reescribimos a su estado original dandole nuevamnete deploy al primer estado, esto como precaucion de que el config no vaya con la configuracion equivocada a PRD. Luego podemos dar deploy al estado dev validation dado que validamos usando el validador y ejehcutamos la rutina.

6. **Release: CER Reception** Entregamos a certifdicacion. certificacion nos devuelve la rutina en VSTS Pre-validation. Enviar correo promove objetos. En caso de que se haya trabajado con una HU de revision, no olvidar que se sale a prd con la HU de calendarizacion.
7. **Release: VSTS Pre valdiation:** Simeplemente debemos desplegar. ellos haran varios checqueos.
8. **Release: Create OC** hablamos encargado de la OC. para indicarle que ya tenemos una rutina hia. le pasamos el release. el certificador cierra el testplan.  Una vez este aprobado debemos veritficar el DoD, el DoD debe tener la Oc asignado de manera correcta y entrar a uno de los estados del release y ver que si se relaciono con el DoD correcto. Hasta la fecha se debe hablar con Luis
9. **Release: DeployPDN** el certificador manda un correo y en infra ejecutan el despliegue en prd.
10. **Copy of Execute Auto Inventory Routine Test:** Revisar el word [Uso de Autoinventario de rutinas](https://bancolombia-my.sharepoint.com/:w:/r/personal/andorreg_bancolombia_com_co1/Documents/Archivos%20de%20chat%20de%20Microsoft%20Teams/Uso%20de%20Autoinventario%20de%20rutinas.docx?d=wa48f70f231264e2b8094c9ad0a3596f2&csf=1&web=1&e=QCr4zq) para mas detalle

  - **Determinar nombre del proceso:** Entrar a DeployUCD para determinar el nombre del proceso con el cual documentaremos la rutina y quedara registrada en PRD y en malla.

  - **Documentacion**
    la rutina debe estar en http://wikiti/wikiti/index.php/CapacidadesAnaliticas-DT
    wikiTI y matriz de riesgos.
    Si la rutina pasa las pruebas -> hacer la documentacion wikiti y sjarepoint.
    si la rutina es de modificacion puede que se requeriera hacer modificaciones en una pagina de la wikti ya creada.
    tomar como ejemplo SQPME1929RUTCOMI.
    La wikiti se llena muy similar a la wiikiti de ingestiones.
    Tener en cuenta que en la wiki se debe cambiar el grupo usd.


11. **Release:user Valdiation.**

    - **Docuemtnar HU:** Poner el nombre del proceso
    
    - **Prueba controlada:** En el asistente en prd ejecutamos el comando en servicios, en ejecutar programa.
    comando: xxxxx
      
          $ cd /home/svchad02/scripts/pyEnvs
          $ nohup python3 /home/<user_name>/AW1003001_BigDataCompany/scripts/pyEnvs/runPyEnv.py --resultDir <results_zone> --pyEnvPkg <package_name> --pySitePck <site-package-name> &
      
        Puede que no finalice ok. asegurarnos de que el correo que llega el orquestador haya finalizado si no, hablar con el usuario. Por posible bug es mejor cerrar la pestaña del asistente cuando se de al boton ejecutar. Para corroborar que todo se esta ejecutando podemos usar los siguientes comandos.

        - revisar logs:
          
              $ cat /avirtual/<zona-resultados>/<paquete>/venv/logs/<log-filename>

        - verificar si la rutina se esta ejecutando en PRD

              $ ps -fea | grep "<zona_resultados>"

        **Nota:** Si falla y ya estaba calendarizada hay que crear una Oc para cancelar la ejecucion.


    
    - **Calendarizar** si la prueba es exitosa insribir en malla. en el excel. 
    llenar excel
    revision par.
      - Si se trata de una rutina virtual va en el servidor xxx, en caso de que sean rutinas tradicionales van en el servidor `sbmdeblzw001`
      - Si se trata de una rutina con periodicidad intradiaria no debe tener algun prerrequisito.

    ### donde se da resume??

11. **Cerrar HU**
    ### como cierro la HU?
    - Verificar el archivo Documetnacion por tipoligia de Hu - cierra en la carpeta de gestion de historias de usuario donde esta la info de que cosas se deben agregar en le cierre de la HU

- 
# TODO
- Validador de rutinas valida solo sql o sql y python.
- Si es solo modificacion, que tanto cambia el proceso? a que si es una nueva?
- Que tanto cambia el desarollo entre una rutina de modificacion y una nueva?
- en el estado de DeployDEv, no entiendo por que hacemos el proceso manual, luego el proceso DevOs no hace todo eso de ejecutar el validador. porque solo crea el ambiente virtual?
- indicar bien lo de los estados, que se cancela el segundo y que si es revision o modificacion o calendarizaxcion se abandona o se continua
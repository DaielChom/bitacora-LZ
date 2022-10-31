La idea es montar una API de los usuarios y probar que se ejecute en todos los ambientes y que tiene acceso a las tablas. 
La API se montara en AWS con tablas de Dynamo.
Durante un sprint se llevan a cabo varias a HU.
Los primeros pasos constaran de subir la infraestructura, los datos y la API en ambientes preproductivos.

## Documentacion
encontraremos una serie de videos en

USAR COMO REFERENCIA PRECALCULOS....

## Infraestructura.
Se contacara con una Hu de "Despliegue de infraestructura" la cual debe ser la primera que se debe trabajar, dado que para subir los datos (tablas de dynamo) y el API se requiere contar con la estructura en AWS (maquinas y buckets).

- hacer count a las talbas en la zona de resultados indicada en la HU de coordinadores.
- comentar HU con los counts.
- Debemos tener clonado el repositorio Cloud_nu0044001_productizar que es donde se modificaran los templates con los cuales se montara de manera automatica la estructura en AWS.
- Crear una nueva rama con la estructura
    $ git checkout -b 

git checkout -b feature/HU3575813_DespliegueInfraestructuraAPIBAMRiesgosA18

- copiar el directorio xxx cambiandole el nombre por el de la api que vamos a desplegar
- modiicar los archivos 
    - modificar las variables en el azure.yaml 
        - con el nombr de la api donde requiera
        - para los primeros despligues dejar la variable condition en false para los stage de despligeue en qa y en prd, esto con el objetivo de no llevar hasta prd despliegue de infraestrucutas que puedan tener errores. Tener en cuenta que debemos probar una ejecucion en todos los estados.
        
    - modificar template:
        - Paremeters:
            
            dynamo<tablename>api:
                Description: Nombre de tabla de dynamo-xx-xx-api
                Type: String

            Dy<TableName>ApiReadCapacityUnits:
                Description: Capacidades de lectura para la tabla dynamo-xx-xx-api
                Type: List<Number>

            Dy<TableName>ApiWriteCapacityUnits:
                Description: Capacidades de lectura para la tabla dynamo-xx-xx-api
                Type: List<Number>

            glueJob<ApiName>:
                Description: Nombre del job para el proyecto <ApiName>
                Type: String

        - Resources:
            - modificar nombre de la api en s3KeyEncript
            - modiifcar aliasname ne s3KeyEncriptAlias
            - modificar el recurso glueJob<Apiname>:
                - cambiar los campos que sean necesarios. Tener en cuenta que el campo Name hace referencia a la variable creada en parameters.
                - Modificar el valor de MaxConcurrentRuns teniendo en cuenta el valor de coordinadores que se ejecutaran a la vez. (validar con el usuario)
                - Modificar el valor de NumberOfWorkers teniendo en el tamaño de datos que se estaran moviendo en prd.
            - por cada tabla agregar los siguientes recursos. Tener en cuenta la clave de ordenamiento y primaria junto con la forma de utilziar el nombre de la tabla, ya sea camelcase o todas en minusculas.

                Dynamo<Tablename>api:
                    Type: AWS::DynamoDB::Table
                    Properties:
                        SSESpecification:
                            KMSMasterKeyId: !Ref "s3KeyEncriptAlias"
                            SSEEnabled: true 
                            SSEType: KMS   
                        PointInTimeRecoverySpecification:         
                            PointInTimeRecoveryEnabled: true
                        TableName: !Ref dynamo<tablename>api
                        BillingMode: !If [IsDyTablesBillingModeProvisioned, PROVISIONED, PAY_PER_REQUEST]
                        AttributeDefinitions:
                        - AttributeName: "<clave_primaria>"
                            AttributeType: "<S/N>"
                        - AttributeName: "<clave_ordenamiento>"
                            AttributeType: "<S/N>"
                        KeySchema:
                        - AttributeName: "<clave_primaria>"
                            KeyType: HASH
                        - AttributeName: "<clave_ordenamiento>"
                            KeyType: RANGE
                        ProvisionedThroughput:
                            ReadCapacityUnits: !If [ IsDyTablesBillingModeProvisioned, !Select [ 0, !Ref Dy<TableName>ApiReadCapacityUnits ], 0 ]
                            WriteCapacityUnits: !If [ IsDyTablesBillingModeProvisioned, !Select [ 0, !Ref Dy<TableName>ApiWriteCapacityUnits ], 0 ]

                        Tags:
                            - Key: 'bancolombia:datos-personales'
                                Value: 'clientes'

                            - Key: 'bancolombia:dominio-informacion'
                                Value: 'productos-canales'

                            - Key: 'bancolombia:cumplimiento'
                                Value: 'no'

                            - Key: 'bancolombia:clasificacion-confidencialidad'
                                Value: 'confidencial'

                            - Key: 'bancolombia:clasificacion-integridad'
                                Value: 'impacto tolerable'

                            - Key: 'bancolombia:clasificacion-disponibilidad'
                                Value: 'impacto moderado'

                            - Key: 'bancolombia:bkmensual'
                                Value: 'no'

                            - Key: 'bancolombia:bkdiario'
                                Value: 'no' 

                Dynamo<Tablename>apiReadScalableTarget:
                    Type: AWS::ApplicationAutoScaling::ScalableTarget
                    Condition: IsDyTablesBillingModeProvisioned
                    Properties:
                        ResourceId: !Sub table/${Dynamo<Tablename>api}
                        ScalableDimension: dynamodb:table:ReadCapacityUnits
                        MinCapacity: !Select [ 0, !Ref Dy<TableName>ApiReadCapacityUnits ]
                        MaxCapacity: !Select [ 1, !Ref Dy<TableName>ApiReadCapacityUnits ]
                        RoleARN: !Sub arn:aws:iam::${AWS::AccountId}:role/aws-service-role/dynamodb.application-autoscaling.amazonaws.com/AWSServiceRoleForApplicationAutoScaling_DynamoDBTable
                        ServiceNamespace: dynamodb

                Dynamo<Tablename>apiReadScalingPolicy:
                    Type: AWS::ApplicationAutoScaling::ScalingPolicy
                    Condition: IsDyTablesBillingModeProvisioned
                    Properties:
                        PolicyName: Dynamo<Tablename>apiReadScalingPolicy
                        PolicyType: TargetTrackingScaling
                        ScalingTargetId: !Ref Dynamo<Tablename>apiReadScalableTarget
                        TargetTrackingScalingPolicyConfiguration:
                            TargetValue: 80
                            PredefinedMetricSpecification:
                                PredefinedMetricType: DynamoDBReadCapacityUtilization

                Dynamo<Tablename>apiWriteScalableTarget:
                    Type: AWS::ApplicationAutoScaling::ScalableTarget
                    Condition: IsDyTablesBillingModeProvisioned
                    Properties:
                        ResourceId: !Sub table/${Dynamo<Tablename>api}
                        ScalableDimension: dynamodb:table:WriteCapacityUnits
                        MinCapacity: !Select [ 0, !Ref Dy<TableName>ApiWriteCapacityUnits ]
                        MaxCapacity: !Select [ 1, !Ref Dy<TableName>ApiWriteCapacityUnits ]
                        RoleARN: !Sub arn:aws:iam::${AWS::AccountId}:role/aws-service-role/dynamodb.application-autoscaling.amazonaws.com/AWSServiceRoleForApplicationAutoScaling_DynamoDBTable
                        ServiceNamespace: dynamodb

                Dynamo<Tablename>apiWriteScalingPolicy:
                    Type: AWS::ApplicationAutoScaling::ScalingPolicy
                    Condition: IsDyTablesBillingModeProvisioned
                    Properties:
                        PolicyName: Dynamo<Tablename>apiWriteScalingPolicy
                        PolicyType: TargetTrackingScaling
                        ScalingTargetId: !Ref Dynamo<Tablename>apiWriteScalableTarget
                        TargetTrackingScalingPolicyConfiguration:
                            TargetValue: 60
                            PredefinedMetricSpecification:
                                PredefinedMetricType: DynamoDBWriteCapacityUtilization

            - Modificar la variable DependsOn del recurso EKSRolePolicyDynamo. Agregando una lista Dynamo<Tablename>api por cada tabla.
            - Modificar la variable Statement denttro de Properties del recurso EKSRolePolicyDynamo.  Agregando una lista por cada tabla.
                - !GetAtt Dynamo<Tablename>api.Arn
                - !Sub ${Dynamo<Tablename>api.Arn}/index/*
                - !Sub ${Dynamo<Tablename>api.Arn}/stream/* 
            - Modificar la variable DependsOn del recurso GlueRolePolicy. Agregando una lista Dynamo<Tablename>api por cada tabla.
            - Modificar la variable PolicyName del recurso GlueRolePolicy. 
            - Modificar la variable Statement denttro de Properties del recurso GlueRolePolicy.  Agregando una lista por cada tabla.
                - !GetAtt Dynamo<Tablename>api.Arn
                - !Sub ${Dynamo<Tablename>api.Arn}/index/*
            - Modificar la variable Effect del recurso GlueRolePolicy con el nombre del API.

    - Modificar params
    - En params cada vriable de capacidad de escritura y lectura WriteCapacityUnits y ReadCapacityUnits cuenta con un valor que esta parametrizado. Estos valores deben ser creados en VSTS en la seccion de library dentro de releases. Las variables deben ser agregadas en el grupo productizar-variables-informacion-dy-aws. A las capacidaddes de lectura se pone 1,50 y a lectura 1,8000
    
    |name|value|
    |-|-|
    |dy-alter-ctra-score-delta-api-tables-read-cu|1,50|
    |dy-alter-ctra-score-delta-api-tables-write-cu|1,8000|

- hacer el push
    - hacer el git add, commit and push
- En vsts revisamos el pull-request pero no lo creamos, debemos realizar despliegue de la infraestructura sobre nuestra rama en los 3 ambientes, antes de pasar trunk.
- Crear release de despliegue infraestructura en nuestra rama: Cada pypline del repositorio cloud ira en una ruta especifica. Estas rutas especificas son carpetas en camelcase con el nombre del proyecto a API. Cada vez que se crea un pipeline se debe conservar la taxominomia.
    - En VSTS nos dirigimos a pipelines y filtramos por 0044001.
    - Damos en New Pipeline y seleccionamos `Azure Repos Git (YAML)`
    - Seleccionamos el repo Cloud_nu0044001_productizar
    - Seleccionamos `Existing Azure Pipelines`
    - Seleccionamos la rama que creamos, no crear el pipelin sobre trunk. y en Path ponemos la ruta del archivo azure-pipelines.yaml
    - continue
    - Darle a Save. NO a RUN.
    - Dar a los 3 punticos y luego a Move/Raneme. Renombrar la ruta del pipeline.
        - name: Cloud_nu0044001_productizar_<api_name>
        - folder: \Cloud_nu0044001_productizar\<ApiName>
    - Save
- Ejecutar pipeline: Damos a run asegurandonos de seleccionar nuestra ram y no trunk en el menu que se despliega.
    - Cuando salga la solicitud de permisos, apoyarnos en algun compa;ero que haya trabajado en apis y enviarle el pr.
    - Si salen algun error en el pipeline realizar los ajustes necesarios
- revisar en AWS si los recursos y el stack fueron creados de manera satisfactoria.
- Crear Pull Request:
    - vamos a vsts a la seccion d pull request y procedemos a crear el pr.
    - Como titulo ponemos 
        Feature HU<huid>: Descripcion breve de lo que realizaste.
    - asignamos el numero de la HU
    - ponemos el tag de lso ambientes segun la configuracion que se tenga en los condition
        #DEV #QA #PRD
    - damos a create
- aprobacion del PR.
    - Debemos hablar con juanesteban, julian o juan david para aprobar el pr.


    


#### Notas:
- En caso de que se deba borrar el stack se utilizar un pipeline. Las instrucciones se encentran en el siguiente [manual](https://bancolombia-my.sharepoint.com/:w:/r/personal/degalvis_bancolombia_com_co/_layouts/15/Doc.aspx?sourcedoc=%7B4401B68E-8EC9-47C6-8F23-2AF6493304CE%7D&file=Manual%20Eliminar%20CloudFormation%20Stack%20AWS%20(1).docx&action=default&mobileredirect=true)

### Coordinadores
utilizamos un repo distinto, el cual es [NU0044001_ProductizarLaAnalitica](https://grupobancolombia.visualstudio.com/Vicepresidencia%20Servicios%20de%20Tecnolog%C3%ADa/_git/NU0044001_ProductizarLaAnalitica)

- Creamos una rama
    git checkout -b feature/HU3575833_Coordinator_building

- Creamos un directorio con el nombre de la API en camelcase <ApiName>
    mkdir <ApiName>
- se crea una nueva carpeta para la nueva api y se crean las subcarpetar `API` y `TMP_FILES`.
- para en subcarpeta `API` se crea una carpeta `cfg` y se copia y pega un parchivo json de xx. Este archiv continene la parametrizacion para el ccordinador hay que tener en cuenta que si vamos adesplegar un proyecto  con API debemos copiar un arhivo xx de un projecto con API, ya que hay algunos proyectos que no tiene API y su archivo xx es un poco diferente.
- Se debe crear un archivo por cadatabla y se nombre como es el nombre de la tabla en la zona de resutlados. 
- Se deben reemplaza_r los parametros en el json segun correspondan.
- En la carpeta `TMP_FILES` se debe crear un directorio `logs` y dentro un archivo vacio `test.txt`
- hacer pr
- crear pr y mandarlo aprobar con un compañero
- Pipeline:
    - deploy a dev:
        - darle a deploy dev
        - revisar que se haya desplegado en dev en la ruta
        - crear carpeta
            mkdir -p /data01/Cloud/TMP_FILES/<ApiName>
        - ejecutar comando



### Desplegar API

- Refactor de codigo:
    - descargar el zip donde viene el API de django
        - descomprmir
        - en el config.json:
            - quitar las claves
            - cambiar los nombres de las tablas para que las tome de forma dinamica
        - modificar el dockerfile
        - modificar el ~~requirements~~
        - modificar ~~models.py~~, urls. y settings
    - si no hay un archivo db.sqlite3:
        - instalar el ambiente virtual
        - ejecutar python3 manage.py migrate
- versionamiento:
    - repo NU....analitica_MR
    - nueva rama
    - probar en local luego de los cambios?
    - PR
- CI/CD
    - Pipeline pruebas
        - crear carpeta dentro de ProductizarLaAnalitica <ApiName>
        - clonar un pipeline
        - arreglar variables necesarias
        - guardar (no ejecutar) en la carpeta que creamos
    
    
- Release
    - se clona un release
    - arreglar variables
    - agregar replcias y max-replicas
    - ver que en variable groups esten docker_users y docker_images
    - Eliminar y crear nuevos artefactos
        - source: ProductizarLaAnalitica/.. 

- solicitar con juan diego crear el tablero de sonar.

- Ejecucion pipelines:
    una vezz
- Pipeline de revisar pods.

- tablero en [Hygieia](https://devops.apps.bancolombia.com/#/) 
    - Team dashboard | Select widgets | nu0044_Productizar LZ Cloud | <component-name> ex NU0044001_ProductizarLaAnalitica_MR_ms_BamRiesgosA18 |

   


## Pruebas especializadas
- descargar jmeter
- pruebas locales:
    - copiar la carpeta testing de otro desarrollo, y desde jmeter empezar a modiifcar el archivo local.
    - realizar las modificaciones y crear nuevos test para cada query que se deba realziar
        - descargar datos de dynamo
    - ejecutar prueba en local
- pruebas reales:
    - modificar el archivo jmx de manera similar al local, pero manteniendo la parametrizacion.
    - hacer pr
    - crear nuevo release verificando que el artifack de las pruebas especializadas ya es el correcto.
    - se necesitan los datos en especificos:
        - para el test plan y para el release:
    - modificar las variables (agregar replicas y max-replicas)
    - modificar las tareas de smoke test y performance test cer
    - hacer testplans? pasarle al certificador los rnf de cada jmeter
    - empezar a hacer pruebas para que el estado de pruebas pase.


## Salida a PRD:

1. **Excepcionar pruebas E2E:**
    - enviar un correo al jefe:
        Se debe enviar un correo para la aprobacion adjuntanto la Matriz AHP.

          
          Aprobación excepción pruebas E2E

          ===============

          Buen día <jefe>,

          Solicito tu aprobación para excepcionar de pruebas E2E del proyecto ProductizarLaAnalitica del API <API-name> y así poder configurar correctamente el pipeline de despliegue en los diferentes ambientes DEV, QA y PDN. Adjunto la Matriz AHP requerida para realizar la solicitud frente al equipo de pruebas continuas.

          Quedo atento.

    - Hablar con gustavo y solciitarte apoyo para excepcionar las pruebas E2E. Se le debe entregar el release, el tablero de sonar, el trablero de hygeiaiaia y el correo donde se evidencie el aprobado

2. **Monitoreo:**
    - Hay que pedir al usuario los siguientes datos:

    - Se debe crear una HA de monitoreo y enviar un correo. Para ello nos podemos basar en el correo con asunto `Monitoreo - API EcoMovilidad - HA3629938 - Cotización Monitoreo API Productizar ( EcoMovilidad)` y la HA `3629938`
    - Modificar excel y word. Podria crear un script para llenar ese excel feo.

3. **Promover API**
    - Enviar a certificacion. El certificador crea el DoD junto con los Testplan que ya se tenian para las especializadas. Entregarle al certificador la HU, el release y testplan.
    - Trabajar el release.
    - Probar contexion.

4. **Promover coordinador**
    - Reservar consecutivo: (para automatizar se podria poner en el release del coordinador)
        
        Hay que reservar un consecutivo por cada coordinador. Es decir por cada tabla.

        Para verificar los coordinadores en el inventario podriamos usar la sigueinte query
              
          $ SELECT package,table_name,semilla,proceso,origen_fuente,data_folder,periodicidad,descripcion,solicita_ingestion,dia_ejecucion,hora_ejecucion,prerrequisito FROM resultados.inventario_lz WHERE proceso LIKE '%CRD%' LIMIT 10

        Para determiar el consecutivo 

          $ select cast(REGEXP_REPLACE(package,'[^0-9]+', "") as int) as consecutivo, package, proceso, count(*) from resultados.inventario_lz group by 1,2,3 order by 1 DESC limit 10;
        
        Tomar el ultimo consecutivo y:

          paquete sqp => SQOOPDI1937 ->  SQP + periodicidad + ultimo consecutivo + 1
          proceso sqp => SQPDI1937CRDCSD -> SQP + periodicidad + ultimo consecutivo + 1 + CRD + Aplicativo

        Reservar

          $ INSERT INTO resultados.Inventario_LZ (package,table_name,semilla,proceso,origen_fuente,data_folder,periodicidad,descripcion,solicita_ingestion,dia_ejecucion,hora_ejecucion,prerrequisito) VALUES ('<paquete>','<table-name-resultados>','<zona-resultados>','<proceso>','Coordinador','<ruta-opt-prd>','<periodicidad>','<descripcion>','<hu-user>','<dia-ejecucion>','<hora-ejecucion>','<prerrequisto>'); 

    - Con el usuario verificar los prerrequistos del coordinador ya que se ejecutara en malla.
    - Mandar certificar. entregarle el release al certificador.
    - 

    



     
    

            

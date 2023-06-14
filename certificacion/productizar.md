# Certificacion procesos productizar

## Certificacion Coordinadores

el proceso de certificacion de coordinadores consiste en ejecutar los coordinadores y validar que se haya echo la transferencia de los datos de manera correcta.

- **Verificar informacion:**
  - El desarrollador debe hacernos entrega de la HU y el release de los coordinadores. La HU debe estar a nombre del certificador y debe haber un comentario en la HU que indique el cambio a certificacion.
  - vamos al commit y a la carpeta API/cfg y revisar los coordinadores.
- **Release:**

  - **Recepcion Equipo CER:** Verificamos la recepcion a certificacion agregando el siguiente comentario y luego damos en Resume.

          Se aprueba la recepción del despliegue on premise de productizar

  - **Deploy CER:** Dejamos que el stage se complete de manera automatica.
  - **Manual Test:**
    - Debemos verificar que los coordinadores se hayan desplegado de manera correcta en la ruta del ambiente de CER `/opt/gitDevops/AW1003001_ProductizarLaAnalitica/<ApiName>/API/cfg`.
    - Ejecutar los coordinadores usando el comando

              sudo su - svchad02
              cd /opt/gitDevops/AW1003001_ProductizarLaAnalitica/ProductizarPy3
              nohup python3 coordinator.py /opt/gitDevops/AW1003001_ProductizarLaAnalitica/<ApiName>/API/cfg/<coordinator-name>.json &

            Tener en cuenta que puede que sea necesario ejecutar `coordinatorV2.py` y no `coordinator.py`
    - Verficar los resultados en el directorio  `/opt/gitDevops/AW1003001_ProductizarLaAnalitica/<ApiName>/TMP_FILES/logs`. Se deben tomar pantallazos delos logs ya que son las evidencias.
    - Generar `testplan`.
    - Generar `DoD`.
    - Resume y comentario de Evidencias VSTS

## Certificacion API

Unos de los procesos de certificacion cuando se trabaja con HUs de API es la de certificar la salida a produccion del API. Es decir llevar a produccion la Imagen del API. Para ello vamos a requerir de la HU, el release y los testplans creados para las pruebas especializadas.

- **Tomar evidencias:** Entramos a los `acceptance test` que tiene el `release`, luego a `gradlew test`. Mirar que diga TEST PASSED y tomar pantallazo de eso, serian dos pantallazos. El de acceptance test cert, y acceptance test dev. Anexar eso a el testplan y listo, esas son las validaciones.
- **Crear DoD:**
- **Trabjar Release:** Comentar en Security E2E

      Evidencia VSTS: XXXX
- Mandar a probar el DoD, entregar nuevamente al desarrollador.

## Pruebas funcionales de API

**Setup:**

Lo primero para poder realizar las pruebas funcionales de api, es que debes de tener las siguientes herramientas:

- Tener instalado Intellij IDEA <https://www.jetbrains.com/idea/download/#section=windows>

- Tener configurado Gradle, si no sabes como acá hay un link <https://docs.spongepowered.org/5.1.0/es/plugin/workspace/idea.html>

- Tener configurado Java y Java JDK 8 en windows <https://docs.appian.com/suite/help/22.1/developer-setup.html>

Una vez configurados estos items, en tu ruta local(no servidor banco) clonas el repo de  ProductizarLaAnalitca_Test: <https://grupobancolombia.visualstudio.com/Vicepresidencia%20Servicios%20de%20Tecnolog%C3%ADa/_git/NU0044001_ProductizarLaAnalitica_Test>

Una vez clonado el repo siempre debes de abrir el intellij desde está carpeta:

![ruta del proyecto](/certificacion/imgs/ruta.png)

Allí encontrarás diferentes archivos con una extension .params estos archivos son las diferentes pruebas que se han realizado.

**Configuracion config2.properties :**

 En la ruta anteriormente mencionada encontrarás un archivo de configuración con está taxonomia:

```
#-----------------------Certified-----------------
Impala.trustStore = src/test/resources/certificados/jssecacerts
Impala.trustStorePassword = changeit
#Impala.trustStorePassword = passw0rd
#---------------------Conexión Impala--------------------
#Certi
#Impala.Conection = jdbc:impala://10.8.71.91:21050/;AuthMech=3;SSL=1;
#Dev
#Impala.Conection = jdbc:impala://10.8.85.237:21050/;AuthMech=3;SSL=1;
Impala.JDBCDriver = com.cloudera.impala.jdbc41.Driver

#---------------------Api Valuespwd consume------------------
usuario = <usuariobanco>@ambientesbc.lab
contra = <contraseña_ambientes_bc>
impala = jdbc:impala://10.8.71.91:21050/;AuthMech=3;SSL=1;

```

Debes de colocar tu contraseña de ambientes bc y tu usario en las casillas  correspondientes

Luego escoger en que ambiente deseas hacer la certificacion

Finalmente guardas el archivo

**NOTA IMPORTANTE :** ❗
Bajo ninguna circustantancia debes de promover el archivo **confi2.properties**

**Creacion Prueba funcional**:

Dentro de la carpeta testing params crear el archivo de prueba con un memotecnico a la api,este debe de ser diferentes a los demas archivos existentes

Una vez creado, abrimos el archivo

debe de tener los siguientes grupos de parametros

````
groups.test = <Nombre_tabla1>,<Nombre_tabla_N>,...
apiId = x-apigw-api-id
apiIdValue = mlnaa6pyka
apiIdKey = x-api-key
apiKeyValue = 54zsbIz6Ao97mAPaLJUon7o4H2Vkq4sl5FVBN7gG
apiContentIdType = Content-Type
apiContentTypeValue = application/json
````

En el parametro `groups.test` van los nombres memotecnicos propuestos por ti para la prueba

Luego:

```
<Nombre_tabla>.query.base =  
<Nombre_tabla>.query.compare =
<Nombre_tabla>.query.replace = 
<Nombre_tabla>.base.endpoint = 
<Nombre_tabla>.replace.endpoint =
<Nombre_tabla>.type.endpoint = POST
<Nombre_tabla>.body.endpoint = 
<Nombre_tabla>.body.endpointreplace = 
<Nombre_tabla>.base.definition.compare =
```

El campo `query.base`  se debe de colocar el query  seleccionado una PK(primary_key) de la db a certifcar, siempre debe tener limit 1 y pueden haber mas de una PK.

e.g:
`select id_cliente from  resultados_bana_vp_riesgos.delta_estimador_ingresos_final limit 1`

El campo `query.compare` puede ser obtenido de ejecutar un archivo de python llamado `concatenate.py` y siempre debe de tener una clausula where con la PK

e.g: `select ingestion_year, ingestion_month,  ingestion_day,id_cliente, clasificacion, ing_final,antiguedad_laboral_final,lugar_trabajo_final,ingreso_solicitudes, antiguedad_laboral_solicitudes,lugar_trabajo_solicitudes,flag_solicitudes,ing_planilla, antiguedad_laboral_planilla, lugar_trabajo_planilla, activo,migrar from resultados_bana_vp_riesgos.delta_estimador_ingresos_final where id_cliente=~`

El campo `query.replace` es el comodín por el cual se va a reemplazar la(s) PK
Ten en cuenta que este comodín deben de ser caracterés espeaciales

algunos de los habitualmente usados son : **~**, **&**, **#**
y algunas subvariantes como **~~**, **&&**, **##**, **~&**, **&#**

**Nota** ❗ : 

No existe ningún problema si se repite el comodín en las diferentes pruebas, sin embargo si en una prueba tienes mas de 1 PK debes de usar comodines diferentes.

e.g: `~=id_cliente`

El campo `base.endpoint` hace referencia al endpoint  donde está la tabla con los datos para hacer pruebas

e.g: `https://informacion-external-qa.apps.ambientesbc.com/productizar/riesgosmodevaestimador/graphql`

El campo `replace.endpoint` siempre se debe de dejar en blanco

El campo `type.endpoint ` siempre debe ser tipo **POST**

El campo `body.endpoint` Este campo se suele sacar de los PR de la API
o en los comentarios de la HU

Para la parte del PR puedes encontarlos  dentro de los archivos de la api
luego la carpeta apy y finalmente en el archivo `queries.json` es importante revisare la cantidad de tablas y debes de centrarte en las que tengan **ALL**

Se copian el quey que esté en formato json , este tiene un comentario al incio que dice `//Json`

Este tendrá unos valores por defecto que puedes validar mendiante insomina,postman usando el enpoint y una petición tipo post para validar que retorne data

e.g: 
```
{
    "query": "query ConsultarEstimadorIngresosAll($idCliente:Int, $limit:Int , $ascendente:Boolean , $lastEvaluatedKey:LastEvaluatedKeyEstimadorIngresos) {estimadorIngresosAll(idCliente:$idCliente, limit:$limit , ascendente:$ascendente , lastEvaluatedKey:$lastEvaluatedKey){totalCount,estimadoringresos{ingestionYear , ingestionMonth , ingestionDay , idCliente , clasificacion , ingFinal , antiguedadLaboralFinal , lugarTrabajoFinal , ingresoSolicitudes , antiguedadLaboralSolicitudes , lugarTrabajoSolicitudes , flagSolicitudes , ingPlanilla , antiguedadLaboralPlanilla , lugarTrabajoPlanilla , activo , migrar}}}","variables": {"idCliente": 9056626}}
```

Luego debes de reemplazarlo por el comdín que vas a usar:

e.g:
```
{"query": "query ConsultarEstimadorIngresosAll($idCliente:Int, $limit:Int , $ascendente:Boolean , $lastEvaluatedKey:LastEvaluatedKeyEstimadorIngresos) {estimadorIngresosAll(idCliente:$idCliente, limit:$limit , ascendente:$ascendente , lastEvaluatedKey:$lastEvaluatedKey){totalCount,estimadoringresos{ingestionYear , ingestionMonth , ingestionDay , idCliente , clasificacion , ingFinal , antiguedadLaboralFinal , lugarTrabajoFinal , ingresoSolicitudes , antiguedadLaboralSolicitudes , lugarTrabajoSolicitudes , flagSolicitudes , ingPlanilla , antiguedadLaboralPlanilla , lugarTrabajoPlanilla , activo , migrar}}}", "variables": {"idCliente":~~}}
```

Finalmente el campo `base.definition.compare` el cual lleva a cabo la comparación de los registros entre impala y dynamo

Está parte suele ser muy critca ya que acá se suelen encontrar la mayoria de los inconvenientes al momento de realizar las pruebas

este campo debe de llevar siempre este formato `data;$PrimeraClave;#0;campodynamo,,campodynamo,tipodatocampo!....`

Se debe de ir llenando así por cada campo de la tabla, cada signo de exclamación hace referencia a la separación de los campos

Para facilitar esta ardua tarea, se realizó un script llamado `concatenate.py`  el cual te facilita la construcción de este campo.


**Ejecución prueba**

Una vez tengamos ambos archivos a la orden del dia se procede a usar el comando de ejecucíon de la prueba este es:

```
Gradle clean test -DrutaParam="<ruta donde tienes el repo>\NU0044001_ProductizarLaAnalitica_Test\Automatizacion\testingParams\<nombrePrueba>.properties" -DrutaConfig="<ruta donde tienes el repo>\NU0044001_ProductizarLaAnalitica_Test\Automatizacion\testingParams\config2.properties" --tests co.com.bancolombia.certificacion.productizar.runners.CompareRegister aggregate -i 
```
debes de tener en cuenta que las rutas deben de ser absolutas es decir, debes de usar el absolute PATH

El comando debe de ser ejectuado dentro de la  ruta `\NU0044001_ProductizarLaAnalitica_Test\ProductizarLaAnalitica` 

el programa se deberá de ejecutar y debe de tener este resultado

![inica_test](/certificacion\imgs\test_started.png)

![finaliza_test](/certificacion\imgs\test_passed.png)


Si el test es exitoso  la prueba está correcta

**Promover la prueba**

- Crear una rama referente a la HU dentro del repo 

- agregas el archivo de prueba solamente

- haces el commit y push

- Crear el PR 

- Una vez creado el pr debemos de crear  un release con el artefacto  mas actual

- Editamos el release y colocamos nuestros datos

- colocamos nuestras credenciales de ambientes bc

- corremos las pruebas E2E 

Una vez pasen de forma exitosa entrar al stage de gradlew build y tomar pantallazos donde se evidencie la prueba exitosa

pegar el pantallazo en la HU  y cerrar.


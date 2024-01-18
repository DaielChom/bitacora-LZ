# Rutinas en CDP PC

 ###:warning: Consideraciones a  tener en cuenta:
      - Una vez se decido ejecutar rutinas en nube, estás se deben de dejar de ejecutar en onprem
Rutinas: Para realizar la Migración de la rutina de on-premises a CDP PC Se requieren realizar los siguientes cambios:

Realizar un cambio en el config.json de las rutinas como se muestra a continuación:

1. Realizar un cambio en el `config.json` de las rutinas como se muestra a continuación:
    
    ```
    {
      
        "global" : {
                "dsn" : "Impala-cdp-kudu",
                "cdp_endpoint" : ""https://bancolombia-de-prd01-manager1.bco-cdp.i5u2-f1yr.cloudera.site/bancolombia-de-prd01/cdp-proxy-api",
                "username" : "srv_svccdp03"
    }
    
    ```
 Para el tema de los logs de estabilidad / compilación , estos deberán de ejecutarse como se hace regularmente, incluyendo el DNS con el cual lanzan las pruebas, para crear el PR si es necesario tener en cuenta los cambios mencionados anteriormente.

2. Disponibilización de las tablas necesarias para las rutinas: En este punto debemos asegurarnos de que la rutina que deseamos migrar a la nube cuente con las suficientes tablas para correr correctamente la rutina.

    Para lograr esto, se debe crear una HU con antelación para subir las tablas a CDP PC a la cuenta de modelos analíticos y recrear las tablas en S3
    Es importante aclarar que si las tablas son de crudos o resultados, estas serán atendidas por 2 equipos diferentes:


    **DATOS CRUDOS** ———> Se encargará el equipo de Analítica
    
    **DATOS RESULTADOS** ———> Se encargará el equipo de la LZ
    
    Para el caso de los datos de resultados, se debe expresar qué cantidad de información se requiere en la nube:
    
    Algunas opciones son:
    
    - ULTIMA PARTICIÓN ———-> Data más nueva.
    - HISTORICOS ———> Data histórica de la tabla, partiendo desde su principio excepto los últimos X días.
    - TABLA SIN PARTICION ->Se Carga toda la tabla a nube

3. Pull_requests:


    Para el caso de los pull_request, se seguirán trabajando como se ha estado realizando en la calendarización 2.0 de rutinas.
4. Releases en la nube:

    Ya no se realizará el despliegue en `AW1003001_CDH_PyEnvs`. Ahora se realizará en 
    `NU0280001_CDP_Public_Cloud_PyEnvs` ——> [VSTS_CDP_PUBLIC_CLOUD](https://grupobancolombia.visualstudio.com/Vicepresidencia%20Servicios%20de%20Tecnolog%C3%ADa/_git/NU0280001_CDP_Public_Cloud_PyEnvs?path=/&version=GBtrunk)


A continuación, se presenta un modelo operativo en BIG-Picture:

![rutinasCDPPC/RTCDP.png]

A continuación, se adjunta un paso a paso para desplegar una rutina en el pipeline de nube: 

https://scribehow.com/embed/How_to_Create_and_Deploy_Release-40__Fj8p8kXXQDuTfLRbCthIXQ

# Rutinas con Spark en CDP Public Cloud


## Pasos a seguir

#:exclamation:NOTA:  :warning: Con el  `config.json` Ya se suplen ambas necesidades tanto para rutinas de impala y con spark:warning:
Con la implementación de la libreria sparky_cdp, dicha clase no es mas que una herencia de la clase original por lo que se conservan los mismos metodos e información de la clase  



Podemos ver  la referencia de la clase en el siguiente link

https://grupobancolombia.visualstudio.com/Vicepresidencia%20de%20Innovaci%C3%B3n%20y%20Transformaci%C3%B3n%20Digital/_git/vrgo-sparky-bc?path=/docs/_build/markdown/index.md


## Uso de la libreria

La clase `SparkyCDP` es una subclase de la clase `Sparky` y proporciona métodos para interactuar con Spark y HDFS. Permite enviar trabajos de Spark, subir y bajar archivos a/desde HDFS, y crear tablas en la Zona de Aterrizaje (LZ) usando varios formatos de archivo.

Ejemplos de uso:

```python
# Crear una instancia de LivySparky
sparky = SparkyCDP(usuario="user", dsn="dsn", endpoint_cdp="cdp_endpoint")

# Enviar un trabajo de Spark
sparky.enviar("script.py", opciones_spark="--executor-memory 4g")

# Subir un archivo a HDFS
sparky.subir_archivo("archivo_local.txt", "/usuario/archivo_hdfs.txt")

# Bajar un archivo de HDFS
sparky.bajar_archivo("/usuario/archivo_hdfs.txt", "archivo_local.txt")

# Crear una tabla en la LZ a partir de un archivo CSV
sparky.subir_csv("datos.csv", "mi_tabla")

# Ejecutar consultas SQL desde un archivo
sparky.ejecutar_archivo_sql("consultas.sql")
```

Funcionalidades Principales:

- Enviar trabajos de Spark usando Livy
- Subir y bajar archivos a/desde HDFS
- Crear tablas en la LZ usando varios formatos de archivo
- Ejecutar consultas SQL desde un archivo

Métodos:
- `submit(archivo, spark_options="", application_arguments="", compress="infer")`:  Envía un trabajo de Spark a Livy.
- `subir_archivo(path_local, path_remote, compress="infer")`: Sube un archivo a HDFS.
- `bajar_archivo(path_remote, path_local)`: Baja un archivo de HDFS.
- `subir_csv(path, nombre_tabla, zona="proceso", spark_options="", compress="infer", modo="error", particion=None, **kwargs)`: Crea una tabla en la LZ  a partir de un CSV.
- `subir_parquet(path, nombre_tabla, zona="proceso", spark_options="", compress="infer", modo="error", particion=None, **kwargs)`: Crea una tabla en la LZ  a partir de un Archivo Parquet.
- `subir_df(df, nombre_tabla, zona="proceso", spark_options="", compress="infer", modo="error", particion=None, parquet=False, **kwargs)`: Crea una tabla en la LZ  a partir de un DataFrame de Pandas.
- `ejecutar_archivo_sql(path, params=None, max_tries=2, spark_options="")`: Ejecuta consultas SQL desde un archivo.

Campos:

- url_livy: La URL del servidor Livy.
- usuario_livy: El nombre de usuario para la autenticación en Livy.
- contraseña_livy: La contraseña para la autenticación en Livy.
- web_hdfs: La URL del servidor WebHDFS.
- registrador: Una instancia de la clase Logger para registrar.
- max_intentos: El número máximo de intentos para ejecutar un comando.
- cliente: Una instancia de la clase Cliente para interactuar con HDFS.

# TestPlan
- **last_update:** 2022-09-02

## Proceso
- **Crear item:**
	- Entramos a la HU que estamos trabajando.
	- Accedemos al padre de dicha HU
    - En related work le damos en new item.
    - En la nuva ventana en work item type le damos en plan Evidencia Pruebas.
    - Agregamos un titulo con formato `TEST PLAN <EVC>_<HUid> SPRINT <sprint_num>`. Ejemplo **TEST PLAN EVC00037_3345791 SPRINT 144**
	- Finalmente damos OK.
        
- **Completar Test plan:**
	- En tipo de test plan seleccionamos `Ambos`
    - Agregamos la siguiente descripcion.
        
          Alcance:
          Se requiere realizar la rutina <rutina-name>.
            
          Historias de usuario:
          HU<HUid> <HU-name>
          AW1003001_CDH_PyEnvs
          Release <release>
            
          Líneas impactadas:
          N/A
            
          Equipo de trabajo:
          Scrum Master:  Nadia Julieth Collazos
          Product Owner: Yamile Onedy Buritica Morales 
          Transformación: N/A
          Disponibilidad del Servicio: N/A
          Seguridad Corporativa: Julio Cesar Perez Calderón
          Certificación Funcional: César Augusto Vélez, Fabian Alexander Pineda Valencia, Andrés Felipe Molina Callejas.      
            
          Estrategia
          Se verifica el correcto despliegue de los objetos en ambiente de certificación y el cumplimiento con los lineamientos para el paso a producción (Lista de chequeo y Matriz de riesgos), sin embargo, no es necesario la ejecución de pruebas por parte del equipo de certificación ya que es un script de usuario funcional(del negocio).
            
          Supuestos y limitaciones:
          Supuestos:
          1. Se cuenta con el ambiente de pruebas estable y controlado, alineado al ambiente de producción.
          2. Se cuenta con los usuarios y accesos a los diferentes recursos.
            
          Limitaciones:
          1. Ambiente inestable y con fallas, para la ejecución de las pruebas.
          2. Falta de acceso al ambiente de pruebas.
          3. Solución de errores tarde más de lo establecido, retrasando las pruebas.

    - Poner un tag que sea AW1003001_CDH_PyEnvs
    - Modificar area e iteration. Agregando la celula y sprint correspondientes a la HU que se esta trabajando
    - Relacionar la HU como item existente. En `link type` poner `relate`, en `work item` el numero de la `HU` y dar OK.
	- Asignarnos el test plan.

-**Plan Pruebas Perfomance:**
	- EN codigo de aplicación y Nombre del Aplicativo/Solución se obtienen desde este link:
	[Inventario MAC](https://bancolombia.sharepoint.com.mcas.ms/:x:/r/sites/co-tst/_layouts/15/Doc.aspx?sourcedoc=%7BA3EFF6A7-E8AC-4726-8DEA-7374A65B7A82%7D&file=Inventario%20MAC%20-%20Resumen.xlsb&action=default&mobileredirect=true&ovuser=b5e244bd-c492-495b-8b10-61bfd453e423%2Cfapineda%40bancolombia.com.co&SafelinksUrl=https%3A%2F%2Fbancolombia.sharepoint.com%2F%3Ax%3A%2Fr%2Fsites%2Fco-tst%2F_layouts%2F15%2FDoc.aspx&cid=c4934860-48d9-4231-9a0c-184792971c30&clickparams=eyJBcHBOYW1lIjoiVGVhbXMtRGVza3RvcCIsIkFwcFZlcnNpb24iOiIxNDE1LzIzMDYwNDAxMTM4IiwiSGFzRmVkZXJhdGVkVXNlciI6ZmFsc2V9)
	
- **Checklist:**
    - Nos dirigimos a checklit
    - En EVC poner `EVC - DATOS ANALITICA E IA`
    - En codigo apliccion poner AW1003001
    - Dar true en la preguta de pruebas continuas de seguridad y luego etiquetar a Cesar Augusto Ospina Jimienez (esto pued ecambiar en el futuro)
    - En el campo de analista de seguridad n la transformacion poner a Julio Cesar Perez Calderón
    - Y finalmenete guardamos.
    - Volvemos a entrar al testplan y a checklist.
    - En `Se está evaluando la necesidad de pruebas de performance modulares o pruebas de performance E2E?` poner `modular NO isieries`
    - Luego en el Tipo de transaccion selecionar Batch.
	
	- **Completar batch:**
        - Damos la opcion de nuevo batch
        - Llenamos el formaulario con la siguiente informacion:
 
            |||
            -|-
            |nombre|Pr_ nombre HU sin guiones|
            |cantidad maxc registros|1 archivo|
            |tamaño maximo del archivo|10mb|
            |tiempo|5min|
            |porcentaje de error|3|
            |crecimineto|10|
            |se ejecuta en malla|No|
            |proceso de la lz|Si|
                
        - Bajamos a RNF batch y verificamos los datos que acabamos de ingresar. Debemos asegurarnos de que Recomienda pruebas este en NO.  Va salir que neecesitamos pruebas E2E, sin embargo estamos libre de eso y debemos adjuntar ciertos archivos en el testplan.
        - Luego damos en evaluar checklist y OK
        Despues de haber dado OK no podemos hgacer cambios, si lo requerimos, debemos mandar a borrar el testplan. 
        verificar que diga que La prueba Modular está a cargo de la célula de trabajo
    - **Completar Infromacion Tecnica Seguridad:**
        - En informacion de seguridad seleccionamos: proyectos.
        - salidas a produccion: mas de 5
        - Cambio que se esta evaluando: otro
        - Pruebas continuas de seguridad: NO
        - El analista de seguridad ha solicitado prueba: NO
        - Luego evaluar. Debe arrojar No se requieren prueba de Seguridad.

- **Adjuntar documentacion:** Debemos adjuntar a la docuemtnacion:
    - El correo que no hacemos pruebas.
    - Se adjunta la matriz de riesgo (adjuntar el execel) luego de averla diligenciado. cambiando las fechas (las dos primeras dos dias antes de la salida a PRD y la ultima el dia de la salida), nombre de usuario, dejar en 1 el riesgo.
    - Los logs de la rutina. en un archivo zip de evidencias con nombre `Evidencias_<EVC>_HU_<HU-id>.zip`
    - otros dos archivos

- agregar comentatio
 - se agrga un comentario tageando @paola andrea molina Rueda
        
- Dejamos el testplan en active.
- Cuando este en createOC se debe cerrar

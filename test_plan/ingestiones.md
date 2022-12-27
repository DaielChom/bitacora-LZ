# TestPlan
- **state:** Building
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
	- En tipo de test plan seleccionamos Ambos
    - Agregamos la siguiente descripcion.
        
		  Alcance:
		  <Alcance que tienen las pruebas que se anexan> (Validar la metadata, calidad de los datos, etc.)
			
		  Historias de usuario: 			
		  HU<HU_id> <HUfullname>
		  AW1003001_BigDataCompany
		  <artifact>
		  <release>
			
		  Líneas impactadas: 
		  N/A
			
		  Equipo de trabajo:
          Scrum Master: Jose Daniel Respreto Uran
          Product Owner: Yamile Onedy Buritica Morales
          Transformación: N/A
          Disponibilidad del Servicio: N/A
          Seguridad Corporativa: Julio Cesar Perez Calderón
          Certificación Funcional: Fabian Alexander Pineda Valencia, Juan David Potes Medina

          Supuestos y limitaciones:
          Supuestos:
          1. Se cuenta con el ambiente de pruebas estable y controlado, alineado al ambiente de producción.
          2. Se cuenta con los usuarios y accesos a los diferentes recursos.
            
          Limitaciones:
          1. Ambiente inestable y con fallas, para la ejecución de las pruebas.
          2. Falta de acceso al ambiente de pruebas.
          3. Solución de errores tarde más de lo establecido, retrasando las pruebas.

       

    - Poner un tag que sea AW1003001_BigDataCompany
    - Modificar area e iteration. Agregando la celula y sprint correspondientes a la HU que estamos certificando.
    - Relacionar la HU como item existente. En `link type` poner `relate`, en `work item` el numero de la `HU` y dar OK.
	- Asignarnos el test plan.
- **Checklist:**
    - Nos dirigimos a checklit
    - En EVC poner `EVC - DATOS ANALITICA E IA`
    - En codigo apliccion poner AW1003001
    - Dar true en la preguta de pruebas continuas de seguridad y luego etiquetar a Cesar Augusto Ospina Jimienez (esto pued ecambiar en el futuro)
    - En el campo de analista de seguridad n la transformacion poner a Julio Cesar Perez Calderón
    - Y finalmenete guardamos.
    - Volvemos a entrar al testplan.
    - En `Se está evaluando la necesidad de pruebas de performance modulares o pruebas de performance E2E?` poner `modular NO isieries`
    - Luego en el Tipo de transaccion selecionar Batch.
	
	- **Completar batch:**
        - Damos la opcion de nuevo batch

            Campo|valor
            -|-
            |nombre|Pr_ nombre HU sin guiones|
            |cantidad maxc registros|1000 registros|
            |tamaño maximo del archivo| 30mb|
            |tiempo| 30min|
            |porcentaje de error| 3|
            |crecimineto| 10|
            |se ejecuta en malla| No|
            |proceso de la lz|Si
        
        - bajamos a RNF batch y verificamos los datos que acabamos de ingresar. ~~debemos asegurarnos de que Recomienda pruebas este en NO~~ Va salir que neecesitamos pruebas E2E, sin embargo estamos libre de eso y debemos adjuntar ciertos archivos en el testplan.

        - Luego damos en evaluar checklist y OK. Despues de haber dado OK no podemos hgacer cambios, si lo requerimos, debemos mandar a borrar el testplan. 
        - Verificar que diga que La prueba Modular está a cargo de la célula de trabajo

    - En informacion de seguridad seleccionamos: proyectos
        salidas a produccion: mas de 5
        cambio que se esta evaluando: otro
        pruebas continuas de seguridad: NO
        El analista de seguridad ha solicitado prueba: NO
        Luego evaluar
        Debe arrojar No se requieren prueba de Seguridad.

    - Debemos adjuntar a la docuemtnacion:
        - Correo que no hacemos pruebas.
        - Se adjunta la matriz de riesgo (adjuntar el execel) luego de averla diligenciado. cambiando las fechas (las dos primeras dos dias antes de la salida a PRD y la ultima el dia de la salida), nombre de usuario, dejar en 1 el riesgo ...
        - las evidencias (logs de prueba controlada). `Evidencias_<EVC>_HU_<HU-id>.zip`
        - otros dos archivos
    - lo dejamos en aactive. Cuando este en deploy PDN se debe cerrar

- agregar comentatio
 - se agrga un comentario tageando @paola andrea molina Rueda
# todo
cuando debe ser cerrado

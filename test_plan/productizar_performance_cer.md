# TestPlan
- **last_update:** 2022-08-31

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

          Alcance
          Testplan para pruebas de performance.

          Historias de usuario:  
          HU<hu_id> | <HU-fullname> | <repository> | <release> | <artifact>


          Líneas impactadas:
          N/A  

          Equipo de trabajo:
          Scrum Master:  Nadia Julieth Collazos
          Product Owner: Yamile Onedy Buritica Morales
          Transformación: N/A
          Disponibilidad del Servicio: N/A
          Seguridad Corporativa: Cristian Camilo Zuluaga
          Certificación Funcional: Fabian Alexander Pineda Valencia, Andres Felipe Molina Callejas, Juan David Potes Medina
          Supuestos y limitaciones:
          Supuestos:
          1. Se cuenta con el ambiente de pruebas estable y controlado, alineado al ambiente de producción.
          2. Se cuenta con los usuarios y accesos a los diferentes recursos.
        
          Limitaciones:
          1. Ambiente inestable y con fallas, para la ejecución de las pruebas.
          2. Falta de acceso al ambiente de pruebas.
          3. Solución de errores tarde más de lo establecido, retrasando las pruebas.
        
            

    - ~~Poner un tag que sea AW1003001_CDH_PyEnvs~~
    - Modificar area e iteration. Agregando la celula y sprint correspondientes a la HU que se esta trabajando
    - Relacionar la HU como item existente. En `link type` poner `relate`, en `work item` el numero de la `HU` y dar OK.
	- Asignarnos el test plan.
- **Checklist:**
    - Nos dirigimos a checklit
    - En EVC poner `EVC - DATOS ANALITICA E IA`
    - En codigo apliccion poner el que viene al incio del nombre del repositorio. Ejemplo `NU0044001`
    - Dar true en la preguta de pruebas continuas de seguridad y luego etiquetar a Cesar Augusto Ospina Jimienez (esto pued ecambiar en el futuro)
    - En el campo de analista de seguridad n la transformacion poner a Julio Cesar Perez Calderón
    - Y finalmenete guardamos.
    - Volvemos a entrar al testplan y a checklist.
    - En `Se está evaluando la necesidad de pruebas de performance modulares o pruebas de performance E2E?` poner `modular NO isieries`
    - Luego en el Tipo de transaccion selecionar Online. Debemos llenar el fomulario por cada transaccion que nos notifique el desarrollador. El valor de cada campo del formulario sera entregado por el desarrollador.
	
	- **Completar Online:**
        - Damos la opcion de nuevo online
        - Llenamos el formaulario con la siguiente informacion:

                -----
                nombre: 
                
                ------
        - Bajamos a RNF Online y verificamos los datos que acabamos de ingresar.
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

- ~~**Adjuntar documentacion:** Debemos adjuntar a la docuemtnacion:~~
    - ~~El correo que no hacemos pruebas.~~
    - ~~Se adjunta la matriz de riesgo (adjuntar el execel) luego de averla diligenciado. cambiando las fechas (las dos primeras dos dias antes de la salida a PRD y la ultima el dia de la salida), nombre de usuario, dejar en 1 el riesgo.~~
    - ~~Los logs de la rutina. en un archivo zip de evidencias con nombre Evidencias_<EVC>_HU_<HU-id>.zip~~
        
- Dejamos el testplan en active.
- ~~Cuando este en createOC se debe cerrar~~

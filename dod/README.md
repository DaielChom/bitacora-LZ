# DOD.
- **state:** Building

## Proceso
- **Crear DoD**
    - Nos dirigimos VSTS al sprint y luego a la celula.
    - Deseleccionamos los filtro que tengamos y le damos a new item DoD.
    - Agregamos el nombre del DoD el cual lleva formato `DOD <EVC> SPRINT <sprint_num> - SALIDA HU<Huid>` Ejemplo. DOD EVC00037 SPRINT 143 - SALIDA HU3227609.
    - Damos en Add to top.    
- **Completar DoD:**
    - Nos asignamos el DoD
    - En la seccio de Concepto de Calidad, donde dice La solucion contiene cambios, darle que No.
    - documentacion de la solucion: si.
    - poner link de la wiki http://wikiti/wikiti/index.php/CapacidadesAnaliticas-DT en Ruta Documantacion
    - luego en calidad de la solucion damos a automatico DevOps
    - en nombre del release: 
        - AW1003001_BigDataCompany
        - AW1003001_CDH_PyEnvs
        - NU0044001_ProductizarLaAnalitica
    - poner luego el artefacto sin el punto final
    - se ejecutaran pruebas especializadas: no
    - ¿Esta solución contiene implementaciones solicitadas por el analista de seguridad?: NO

    - En evidencia pruebas poner

          Evidecias VSTS: numero test plan

    - luiego relacionamos la hu como related
    - luego relacionar el mismo padre que tiene la HU
    - guardar
            active
            guardar
            mandar a aprobar el Dod

                ##########################

# todo
que yamile lo apruebe
se cierra, 
condiciones de cuanto tiempo dura la aprobacion, etc
- Si el DoD es aprobado por alguien mas que no sea la PO propia (yamile) en el testplan se deber cambiar el nombre y poner un comentario de que se aprueba con otra PO.

- Ir USD y ver que la OC  (change order) y comprobar que tiene el codigo de la DO. De igual manera la DoD debe tener la OC correcta. Esto sucede cuando el DoD queda mal gestionado
- No puede durar mas de 24h cerrado sin OC

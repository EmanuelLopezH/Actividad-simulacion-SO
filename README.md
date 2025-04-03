# Actividad de seguimiento - Simulación 1

| Integrante           | correo                     | usuario github |
| -------------------- | -------------------------- | -------------- |
| Emanuel Lopez Hiuita | emanuel.lopezh@udea.edu.co | EmanuelLopezH  |
| Andres Calvo         | andres.calvoa@udea.edu.co  | andres-calvo   |

## Instrucciones

Antes de empezar a realizar esta actividad haga un **fork** de este repositorio y sobre este trabaje en la solución de las preguntas planteadas en la actividad de simulación. Las respuestas deben ser respondidas en español o si lo prefiere en ingles en el lugar señalado para ello (La palabra **answer** muestra donde).

**Importante**:

- Como la actividad es en las parejas del laboratorio, solo uno de los integrantes tiene que hacer el fork; y sobre repositorio bifurcado que se genera, la modificación se realiza en equipo.
- Como la entrega se debe hacer modificando el archivo READNE, se recomienda que consulte mas sobre el lenguaje **Markdown**. En el repo adjuntan dos cheatsheet ([cheat sheet 1](Markdown_Cheat_Sheet.pdf), [cheatsheet 2](markdown-cheatsheet.pdf)) para consulta rapida.
- Entre mas creativo mejor.

## Homework (Simulation)

This program, [`process-run.py`](process-run.py), allows you to see how process states change as programs run and either use the CPU (e.g., perform an add instruction) or do I/O (e.g., send a request to a disk and wait for it to complete). See the [README](https://github.com/remzi-arpacidusseau/ostep-homework/blob/master/cpu-intro/README.md) for details.

### Questions

1. Run `process-run.py` with the following flags: `-l 5:100,5:100`. What should the CPU utilization be (e.g., the percent of time the CPU is in use?) Why do you know this? Use the `-c` and `-p` flags to see if you were right.

   <details>
   <summary>Answer</summary>
   100% ya que en los parametros son, el tiempo que se va a correr el proceso y luego, el uso de la cpu en porcentaje, y como los parametros son 100, indica que va a usar solo la CPU
   </details>
   <br>

2. Now run with these flags: `./process-run.py -l 4:100,1:0`. These flags specify one process with 4 instructions (all to use the CPU), and one that simply issues an I/O and waits for it to be done. How long does it take to complete both processes? Use `-c` and `-p` to find out if you were right.

   <details>
   <summary>Answer</summary>
   Primera respuesta: 6, segunda respuesta: 11. Los primero 4 tiempos los usa el primer proceso con la cpu, y los otros dos creiamos que era uno de inicio y otro de hecho, no se tenia conocimiento que en el proceso de IO, se queda 5 tiempos en el estado bloqueado
   </details>
   <br>

3. Switch the order of the processes: `-l 1:0,4:100`. What happens now? Does switching the order matter? Why? (As always, use `-c` and `-p` to see if you were right)

   <details>
   <summary>Answer</summary>
   Primero se corrio el proceso de IO y luego el de CPU, sin embargo, mientras el proceso de IO entro en estado 'BLOCKED', dado que no se puede dejar la CPU sin ejecutar nada, el segundo proceso se corrio y termino, luego de los 5 tiempos en estado 'BLOCKED', el proceso de IO  finalizo, terminando asi en un menor tiempo que el anterior numeral
   </details>
   <br>

4. We'll now explore some of the other flags. One important flag is `-S`, which determines how the system reacts when a process issues an I/O. With the flag set to SWITCH ON END, the system will NOT switch to another process while one is doing I/O, instead waiting until the process is completely finished. What happens when you run the following two processes (`-l 1:0,4:100 -c -S SWITCH ON END`), one doing I/O and the other doing CPU work?

   <details>
   <summary>Answer</summary>
   Ahora empezo el proceso de IO, sin embargo, con la bandera SWITCH ON END, el proceso que usa la cpu, no pudo ser ejecutado mientras el otro estaba en estado de bloqueado, por el contrario, espero que el proceso de IO terminara y poniendo primero el proceso de CPU, ocurre, es lo mismo que no usar la bandera
   </details>
   <br>

5. Now, run the same processes, but with the switching behavior set to switch to another process whenever one is WAITING for I/O (`-l 1:0,4:100 -c -S SWITCH ON IO`). What happens now? Use `-c` and `-p` to confirm that you are right.

   <details>
   <summary>Answer</summary>
   Al realizar el switch cuando se realiza un request de I/O, permite que el PID:1 pueda ejecutar e, incluso, terminar antes de que el request de I/O del PID:0 termine. La ejecución duró 7 vs. 11 unidades de tiempo; con los flags del punto anterior, nos ahorramos 4 unidades de tiempo.
   </details>
   <br>

6. One other important behavior is what to do when an I/O completes. With `-I IO RUN LATER`, when an I/O completes, the process that issued it is not necessarily run right away; rather, whatever was running at the time keeps running. What happens when you run this combination of processes? (`./process-run.py -l 3:0,5:100,5:100,5:100 -S SWITCH ON IO -c -p -I IO RUN LATER`) Are system resources being effectively utilized?

   <details>
   <summary>Answer</summary>
   Consideramos que los recursos no estan siendo utilizados efectivamente, el PID:0 alargo toda la ejecución en espera de poder ejecutar su primer io_done, y luego de esto como ya los otros procesos terminaron, la cpu quedo libre ejecutando solo los io_done y RUN:io restantes del PID:0
   </details>
   <br>

7. Now run the same processes, but with `-I IO RUN IMMEDIATE` set, which immediately runs the process that issued the I/O. How does this behavior differ? Why might running a process that just completed an I/O again be a good idea?

   <details>
   <summary>Answer</summary>
   A diferencia de la ejecución anterior, esta estrategia permite una utilización más eficiente de los recursos. Esto se debe a que el proceso que generó una solicitud de I/O puede reanudarse rápidamente. En este ejemplo, el PID:0 realiza múltiples solicitudes de I/O, lo que facilita detectar y ejecutar nuevas operaciones de I/O pendientes mientras se alterna con otros procesos. Ejecutar nuevamente un proceso que completó una operación de I/O es beneficioso porque podría tener más solicitudes de I/O pendientes, optimizando así el uso del procesador y reduciendo tiempos de espera.
   </details>
   <br>

### Criterios de evaluación

- [x] Despligue de los resultados y analisis claro de los resultados respecto a lo visto en la teoria.
- [x] Creatividad y orden.

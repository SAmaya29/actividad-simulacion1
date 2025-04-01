# 📌 Actividad de Seguimiento - Simulación 1

## 👥 Integrantes
| Nombre | Correo | Usuario GitHub |
|--------|--------|---------------|
| Sebastian Amaya Perez | sebastian.amaya1@udea.edu.co | SAmaya29 |
| Emmanuel Bustamante Valbuena | Emma-Ok |

---


# 🏗️ Homework (Simulation)
Este programa (`process-run.py`) permite observar cómo cambian los estados de los procesos a medida que ejecutan instrucciones en la CPU o realizan operaciones de E/S. Consulte la documentación para más detalles.

---

## 🔎 Preguntas y Análisis

### 🚀 Pregunta 1
Ejecute:
```bash
./process-run.py -l 5:100,5:100
```
📌 **¿Cuál debería ser la utilización de la CPU?** Justifique su respuesta. Luego, use `-c` y `-p` para verificar.

**📝 Respuesta:**

La utilización de la CPU se calcula como el tiempo total que la CPU está ocupada dividido por el tiempo total de ejecución.
```
Utilización de la CPU = (Tiempo CPU ocupada / Tiempo total) * 100%
```
Se obtiene el siguiente resultado:
```plaintext
Process 0
  cpu
  cpu
  cpu
  cpu
  cpu
Process 1
  cpu
  cpu
  cpu
  cpu
  cpu
```

Viendo esta primera respuesta que arroja el script podemos ver que hay dos procesos y que cada uno con 5 rafagas de CPU, primero ejecuta el proceso 0 durante 5 unidades de tiempo y luego el proceso 1 durante 5 tiempos.
Con esto podemos evidenciar que hay una utilizacion del **100%** de la CPU ya que El tiempo total de simulación es 10 unidades y la CPU estuvo ocupada durante todas esas 10 unidades.
```
Utilización de la CPU = (10 / 10) * 100% = 100%
```
Ejecutando el siguiente comando podemos obtener estadísticas mas detalladas:
```bash
./process-run.py -l 5:100,5:100 -c -p
```

Se obtiene el siguiente resultado:
```plaintext
Time        PID: 0        PID: 1           CPU           IOs
  1        RUN:cpu         READY             1
  2        RUN:cpu         READY             1
  3        RUN:cpu         READY             1
  4        RUN:cpu         READY             1
  5        RUN:cpu         READY             1
  6           DONE       RUN:cpu             1
  7           DONE       RUN:cpu             1
  8           DONE       RUN:cpu             1
  9           DONE       RUN:cpu             1
 10           DONE       RUN:cpu             1

Stats: Total Time 10
Stats: CPU Busy 10 (100.00%)
Stats: IO Busy  0 (0.00%)
```

Por lo tanto, los resultados obtenidos con `-c` y `-p` confirman la predicción de que la CPU estuvo completamente utilizada.


---

### 🔄 Pregunta 2
Ejecute:
```bash
./process-run.py -l 4:100,1:0
```
📌 **¿Cuánto tarda en completarse ambos procesos?** Use `-c` y `-p` para confirmar.

**📝 Respuesta:**

Ejecutando el script podemos evidenciar lo siguiente:
```
Process 0
  cpu
  cpu
  cpu
  cpu

Process 1
  io
  io_done

```

Esto define dos procesos:  
- **Proceso 0**: Tiene **4 instrucciones CPU**.  
- **Proceso 1**: Realiza **una operación de E/S (IO) y espera** a que termine.  

#### 🔎 Análisis esperado:

1. **El Proceso 0** usa la CPU durante 4 unidades de tiempo.
2. **El Proceso 1** inicia su operación de E/S después de que Proceso 0 termine.
3. **Proceso 1** permanece bloqueado hasta que su E/S finaliza.
4. El tiempo total de ejecución dependerá del tiempo necesario para completar la E/S.

Ahora ejecutando el siguiente comando podemos obtener estadísticas mas detalladas:
```bash
./process-run.py -l 4:100,1:0 -c -p
```

📊 **Estadísticas del sistema**:  
```
Time        PID: 0        PID: 1           CPU           IOs
  1        RUN:cpu         READY             1
  2        RUN:cpu         READY             1
  3        RUN:cpu         READY             1
  4        RUN:cpu         READY             1
  5           DONE        RUN:io             1
  6           DONE       BLOCKED                           1
  7           DONE       BLOCKED                           1
  8           DONE       BLOCKED                           1
  9           DONE       BLOCKED                           1
 10           DONE       BLOCKED                           1
 11*          DONE   RUN:io_done             1

Stats: Total Time 11
Stats: CPU Busy 6 (54.55%)
Stats: IO Busy  5 (45.45%)
```
- **Tiempo total**: **11 unidades de tiempo**.  
- **Uso de CPU**: 6 unidades de tiempo (**54.55% de utilización**).  
- **Uso de IO**: 5 unidades de tiempo (**45.45% de utilización**).

#### 🔎 Análisis del tiempo total  

1. **El Proceso 0** ejecuta sus 4 instrucciones CPU sin interrupción (t = 1 a 4).  
2. **El Proceso 1** inicia su operación de E/S en t = 5.  
3. La E/S bloquea al Proceso 1 durante **5 unidades de tiempo** (t = 6 a 10).  
4. Finalmente, el Proceso 1 completa su ejecución en **t = 11**.

#### Conclusión  


El tiempo total de ejecución es **11 unidades de tiempo**, ya que la operación de E/S introduce un retraso significativo. Sin esta espera, el sistema habría finalizado en **5 unidades de tiempo**. Esto demuestra cómo las operaciones de E/S afectan el rendimiento y utilización del procesador.


---

### 🔀 Pregunta 3
Ejecute:
```bash
./process-run.py -l 1:0,4:100
```
📌 **¿Qué sucede ahora? ¿Importa el orden de los procesos?** Justifique con `-c` y `-p`.

**📝 Respuesta:**

Cuando se invierte el orden de los procesos, el comportamiento del sistema cambia notablemente. En este caso, el Proceso 0 realiza una operación de E/S al inicio, lo que lo deja en estado bloqueado hasta que la operación finalice. Mientras tanto, el Proceso 1 ejecuta sus cuatro instrucciones en la CPU de manera continua.
Viendo la ejecucion del script evidenciamos lo antes mencionado:
```
Process 0
  io
  io_done

Process 1
  cpu
  cpu
  cpu
  cpu

```
Ahora ejecutando el siguiente comando podemos obtener estadísticas mas detalladas:
```bash
./process-run.py -l 1:0,4:100 -c -p
```

📊 **Estadísticas del sistema**:  
```
Time        PID: 0        PID: 1           CPU           IOs
  1         RUN:io         READY             1
  2        BLOCKED       RUN:cpu             1             1
  3        BLOCKED       RUN:cpu             1             1
  4        BLOCKED       RUN:cpu             1             1
  5        BLOCKED       RUN:cpu             1             1
  6        BLOCKED          DONE                           1
  7*   RUN:io_done          DONE             1

Stats: Total Time 7
Stats: CPU Busy 6 (85.71%)
Stats: IO Busy  5 (71.43%)
```
- **Tiempo total**: **7 unidades de tiempo**.  
- **Uso de CPU**: 6 unidades de tiempo (**85.71% de utilización**).  
- **Uso de IO**: 5 unidades de tiempo (**71.43% de utilización**).

#### Conclusión 


Sí importa el orden de los procesos, porque en este caso, la CPU no se queda inactiva en ningún momento. Al iniciar con una operación de E/S, el sistema aprovecha el tiempo en el que el Proceso 0 está bloqueado para ejecutar el Proceso 1 sin interrupciones. Esto resulta en una mayor eficiencia en la utilización de la CPU, en comparación con el escenario anterior, donde se terminaba el proceso que usaba CPU antes de ejecutar la E/S.


---

### ⏳ Pregunta 4
Ejecute:
```bash
./process-run.py -l 1:0,4:100 -c -S SWITCH ON END
```
📌 **¿Cómo se comporta el sistema cuando un proceso hace E/S y otro usa CPU?**

**📝 Respuesta:**

La bandera -S SWITCH_ON_END hace que el sistema no cambie de proceso mientras el actual no haya terminado por completo, incluso si está bloqueado por una operación de Entrada/Salida (E/S). 
Sabiendo esto vamos a observar la informacion detallada que nos ofrece el script

📊 **Estadísticas del sistema**:  
```
Time        PID: 0        PID: 1           CPU           IOs
  1         RUN:io         READY             1
  2        BLOCKED         READY                           1
  3        BLOCKED         READY                           1
  4        BLOCKED         READY                           1
  5        BLOCKED         READY                           1
  6        BLOCKED         READY                           1
  7*   RUN:io_done         READY             1
  8           DONE       RUN:cpu             1
  9           DONE       RUN:cpu             1
 10           DONE       RUN:cpu             1
 11           DONE       RUN:cpu             1

```

Lo que se observa en la ejecucion es lo siguiente:
1. **El Proceso 0 comienza su ejecución y realiza una operación de E/S inmediatamente.**
   -Dado que SWITCH_ON_END está activado, el sistema no cambia al Proceso 1 mientras el Proceso 0 no termine completamente.
   -Como resultado, la CPU queda inactiva mientras la E/S se completa.
2. **La CPU permanece sin uso durante varios ciclos (tiempos 2-6), esperando que la E/S termine.**
   -Esto es ineficiente, ya que el Proceso 1, que solo usa CPU y está listo para ejecutarse, no puede hacerlo hasta que el Proceso 0 finalice completamente.
3. **Cuando la E/S del Proceso 0 termina (tiempo 7), este finaliza su ejecución.**
   -Solo entonces el Proceso 1 puede comenzar su ejecución en la CPU.
4. **El Proceso 1 ejecuta sus 4 instrucciones de CPU consecutivamente.**

#### Conclusión 


El uso de SWITCH_ON_END en este escenario provoca que la CPU se desperdicie mientras el Proceso 0 espera por su operación de E/S. Esto reduce la eficiencia del sistema, ya que el Proceso 1 pudo haber utilizado la CPU durante ese tiempo muerto.


---

### ⚡ Pregunta 5
Ejecute:
```bash
./process-run.py -l 1:0,4:100 -c -S SWITCH ON IO
```
📌 **¿Qué ocurre ahora? ¿Cómo se compara con el caso anterior?**
```
Time        PID: 0        PID: 1           CPU           IOs
  1         RUN:io         READY             1
  2        BLOCKED       RUN:cpu             1             1
  3        BLOCKED       RUN:cpu             1             1
  4        BLOCKED       RUN:cpu             1             1
  5        BLOCKED       RUN:cpu             1             1
  6        BLOCKED          DONE                           1
  7*   RUN:io_done          DONE             1

```

**📝 Respuesta:**

Inicio del Proceso 0: Este proceso comienza su ejecución y realiza una operación de E/S (entrada/salida) de inmediato.​

Cambio al Proceso 1: Debido a que la opción -S SWITCH_ON_IO está activada, el sistema cambia al Proceso 1 tan pronto como el Proceso 0 inicia su operación de E/S.​

Ejecución del Proceso 1: El Proceso 1, que consta de 4 instrucciones de CPU, se ejecuta de manera continua mientras el Proceso 0 espera que su operación de E/S finalice.​

Finalización del Proceso 1: Una vez que el Proceso 1 completa todas sus instrucciones, el sistema verifica si hay otras tareas pendientes.​

Reanudación del Proceso 0: Cuando la operación de E/S del Proceso 0 termina, este retoma su ejecución y finaliza su tarea.​

Conclusión: Al utilizar la opción -S SWITCH_ON_IO, el sistema permite que otro proceso utilice la CPU mientras uno está esperando una operación de E/S. Esto mejora la eficiencia del sistema al reducir el tiempo en que la CPU permanece inactiva, aprovechando mejor los recursos disponibles.
---

### 🔁 Pregunta 6
Ejecute:
```bash
./process-run.py -l 3:0,5:100,5:100,5:100 -S SWITCH ON IO -c -p -I IO RUN LATER
```
📌 **¿Se están utilizando eficientemente los recursos del sistema?**
```
Time        PID: 0        PID: 1        PID: 2        PID: 3           CPU           IOs
  1         RUN:io         READY         READY         READY             1
  2        BLOCKED       RUN:cpu         READY         READY             1             1
  3        BLOCKED       RUN:cpu         READY         READY             1             1
  4        BLOCKED       RUN:cpu         READY         READY             1             1
  5        BLOCKED       RUN:cpu         READY         READY             1             1
  6        BLOCKED       RUN:cpu         READY         READY             1             1
  7*         READY          DONE       RUN:cpu         READY             1
  8          READY          DONE       RUN:cpu         READY             1
  9          READY          DONE       RUN:cpu         READY             1
 10          READY          DONE       RUN:cpu         READY             1
 11          READY          DONE       RUN:cpu         READY             1
 12          READY          DONE          DONE       RUN:cpu             1
 13          READY          DONE          DONE       RUN:cpu             1
 14          READY          DONE          DONE       RUN:cpu             1
 15          READY          DONE          DONE       RUN:cpu             1
 16          READY          DONE          DONE       RUN:cpu             1
 17    RUN:io_done          DONE          DONE          DONE             1
 18         RUN:io          DONE          DONE          DONE             1
 19        BLOCKED          DONE          DONE          DONE                           1
 20        BLOCKED          DONE          DONE          DONE                           1
 21        BLOCKED          DONE          DONE          DONE                           1
 22        BLOCKED          DONE          DONE          DONE                           1
 23        BLOCKED          DONE          DONE          DONE                           1
 24*   RUN:io_done          DONE          DONE          DONE             1
 25         RUN:io          DONE          DONE          DONE             1
 26        BLOCKED          DONE          DONE          DONE                           1
 27        BLOCKED          DONE          DONE          DONE                           1
 28        BLOCKED          DONE          DONE          DONE                           1
 29        BLOCKED          DONE          DONE          DONE                           1
 30        BLOCKED          DONE          DONE          DONE                           1
 31*   RUN:io_done          DONE          DONE          DONE             1

Stats: Total Time 31
Stats: CPU Busy 21 (67.74%)
Stats: IO Busy  15 (48.39%)
```

**📝 Respuesta:**
el sistema no utiliza eficientemente sus recursos. Cuando un proceso inicia una operación de E/S, el planificador cambia al siguiente proceso listo, lo que permite que la CPU siga activa. Sin embargo, una vez que todos los procesos están bloqueados esperando E/S, la CPU permanece inactiva hasta que una operación de E/S se completa, momento en el cual el proceso correspondiente se coloca al final de la cola de listos. Este comportamiento provoca períodos en los que la CPU no se utiliza, reduciendo la eficiencia del sistema.

---

### 🚀 Pregunta 7
Ejecute:
```bash
./process-run.py -l 3:0,5:100,5:100,5:100 -S SWITCH ON IO -c -p -I IO RUN IMMEDIATE
```
📌 **¿Cómo cambia el comportamiento? ¿Por qué podría ser útil esta estrategia?**
```
Time        PID: 0        PID: 1        PID: 2        PID: 3           CPU           IOs
  1         RUN:io         READY         READY         READY             1
  2        BLOCKED       RUN:cpu         READY         READY             1             1
  3        BLOCKED       RUN:cpu         READY         READY             1             1
  4        BLOCKED       RUN:cpu         READY         READY             1             1
  5        BLOCKED       RUN:cpu         READY         READY             1             1
  6        BLOCKED       RUN:cpu         READY         READY             1             1
  7*   RUN:io_done          DONE         READY         READY             1
  8         RUN:io          DONE         READY         READY             1
  9        BLOCKED          DONE       RUN:cpu         READY             1             1
 10        BLOCKED          DONE       RUN:cpu         READY             1             1
 11        BLOCKED          DONE       RUN:cpu         READY             1             1
 12        BLOCKED          DONE       RUN:cpu         READY             1             1
 13        BLOCKED          DONE       RUN:cpu         READY             1             1
 14*   RUN:io_done          DONE          DONE         READY             1
 15         RUN:io          DONE          DONE         READY             1
 16        BLOCKED          DONE          DONE       RUN:cpu             1             1
 17        BLOCKED          DONE          DONE       RUN:cpu             1             1
 18        BLOCKED          DONE          DONE       RUN:cpu             1             1
 19        BLOCKED          DONE          DONE       RUN:cpu             1             1
 20        BLOCKED          DONE          DONE       RUN:cpu             1             1
 21*   RUN:io_done          DONE          DONE          DONE             1

Stats: Total Time 21
Stats: CPU Busy 21 (100.00%)
Stats: IO Busy  15 (71.43%)
```
**📝 Respuesta:**
Al comparar la ejecución del comando con las opciones -I IO_RUN_IMMEDIATE y -I IO_RUN_LATER, observamos diferencias significativas en el comportamiento del sistema.

Comportamiento con -I IO_RUN_IMMEDIATE:

Cuando un proceso completa una operación de E/S, el sistema cambia inmediatamente a ese proceso, permitiéndole continuar su ejecución sin demora.​

Comportamiento con -I IO_RUN_LATER:

Tras finalizar una operación de E/S, el proceso que la solicitó queda en estado listo, pero el sistema continúa ejecutando el proceso actual hasta que este termine o requiera E/S.​

Conclusión.

Utilización de la CPU: Con IO_RUN_IMMEDIATE, la CPU se mantiene ocupada de manera más constante, ya que los procesos que completan E/S retoman su ejecución de inmediato, reduciendo los tiempos de inactividad.​

Eficiencia general: Al priorizar los procesos que finalizan E/S, se mejora la capacidad de respuesta del sistema, especialmente en entornos donde las operaciones de E/S son frecuentes y los procesos dependen de resultados inmediatos.

---

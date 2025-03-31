# 📌 Actividad de Seguimiento - Simulación 1

## 👥 Integrantes
| Nombre | Correo | Usuario GitHub |
|--------|--------|---------------|
| Sebastian Amaya Perez | sebastian.amaya1@udea.edu.co | SAmaya29 |
| Nombre completo integrante 2 | correo integrante 2 | github user integrante 2 |

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

**📝 Respuesta:**

---

### 🔁 Pregunta 6
Ejecute:
```bash
./process-run.py -l 3:0,5:100,5:100,5:100 -S SWITCH ON IO -c -p -I IO RUN LATER
```
📌 **¿Se están utilizando eficientemente los recursos del sistema?**

**📝 Respuesta:**

---

### 🚀 Pregunta 7
Ejecute:
```bash
./process-run.py -l 3:0,5:100,5:100,5:100 -S SWITCH ON IO -c -p -I IO RUN IMMEDIATE
```
📌 **¿Cómo cambia el comportamiento? ¿Por qué podría ser útil esta estrategia?**

**📝 Respuesta:**

---

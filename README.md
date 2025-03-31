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

---

### 🔀 Pregunta 3
Ejecute:
```bash
./process-run.py -l 1:0,4:100
```
📌 **¿Qué sucede ahora? ¿Importa el orden de los procesos?** Justifique con `-c` y `-p`.

**📝 Respuesta:**

---

### ⏳ Pregunta 4
Ejecute:
```bash
./process-run.py -l 1:0,4:100 -c -S SWITCH ON END
```
📌 **¿Cómo se comporta el sistema cuando un proceso hace E/S y otro usa CPU?**

**📝 Respuesta:**

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

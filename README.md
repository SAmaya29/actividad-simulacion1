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

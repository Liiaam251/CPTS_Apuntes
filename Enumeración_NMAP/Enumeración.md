# Enumeración en Pentesting

La **enumeración** es la fase **más crítica** del pentesting.  
No se trata solo de **acceder a un sistema**, sino de **descubrir todas las posibles vías de ataque**.  
El objetivo es **recopilar la mayor cantidad de información posible** sobre el objetivo para encontrar vulnerabilidades.

---

## 1. Concepto Clave

- **Enumeration is the key**: la enumeración es la **llave para el éxito** en una intrusión.
- No depende únicamente de las **herramientas**, sino de **cómo interpretamos la información**.
- Conocer el **funcionamiento de los servicios y protocolos** nos permite interactuar mejor con ellos.

---

## 2. Objetivo Principal

Encontrar:
1. **Funciones o recursos** que permitan interactuar con el objetivo.  
2. **Información que revele más datos** para seguir avanzando.

> Cuanta más información obtengamos, más sencillo será hallar los vectores de ataque.

---

## 3. Importancia del Conocimiento

- Muchas configuraciones inseguras provienen de **errores humanos o desconocimiento**.
- Un pentester debe **comprender los servicios, su sintaxis y su propósito** para explotarlos de forma eficaz.
- Aprender cómo funciona cada servicio puede **ahorrar horas o días** de trabajo.

---

## 4. Herramientas vs Enumeración Manual

- Las herramientas (como **Nmap**, **enum4linux**, etc.) **aceleran el proceso**, pero no siempre detectan todo.
- **Enumeración manual**:
  - Permite detectar servicios ocultos, mal configurados o lentos en responder.
  - A menudo revela **puertos que las herramientas marcan como cerrados o filtrados** por errores de timeout.


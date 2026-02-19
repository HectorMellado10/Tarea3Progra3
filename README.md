# Tarea 3 – Progra 3 (Maven + Eclipse)  
**Librería local + App consumidora + Pila manual + Validador de expresiones + Ofuscación reproducible + Ingeniería inversa + Pruebas por consola**

---

## 1) Estructura del repositorio
Este repositorio contiene dos modulos independientes

- `umg.edu.gt.data-structure.stack/` → **Librería** (estructura de datos: pila enlazada)
- `stackHandler/` → **Aplicación** (consume la librería y valida expresiones)
- `evidencias/` → Capturas solicitadas (compilación, ejecución, ofuscación, decompilación)

---

## 2) Requisitos
-**Java (JDK):** probado con **JDK 11**.  
  También puede compilar con **JDK 17** siempre que `JAVA_HOME` apunte al JDK instalado y el `pom.xml` mantenga `maven.compiler.release` acorde (11 o 17).
- **Maven:** 3.9.x
- ** JD-GUI para decompilar JARs

Verificación rápida:
~~~bash
java -version
mvn -v
~~~

---

## 3) Parte A – Dependencia Maven entre proyectos

### 3.1 Compilar e instalar la librería local (`umg.edu.gt.data-structure.stack`)
Desde la raíz del repo:
~~~bash
cd umg.edu.gt.data-structure.stack
mvn clean install
~~~

Esto instala la librería en el repositorio local de Maven (`~/.m2`), y luego puede consumirse como dependencia desde `stackHandler`.

### 3.2 Compilar el proyecto consumidor (`stackHandler`)
~~~bash
cd ../stackHandler
mvn clean package
~~~

---

## 4) Parte B – Implementación funcional

### 4.1 Librería: Pila enlazada manual
La estructura de pila fue implementada **sin usar `java.util.Stack`**.  
Incluye operaciones típicas (`push`, `pop`, `peek`, `isEmpty`) y métodos adicionales requeridos:

- `getCount()` / `getSize()` / `length()` → devuelven la cantidad de elementos en la pila
- `getNodeInit()` → devuelve el nodo inicial (fondo) de la pila

### 4.2 Handler: Validador de expresiones (`SymbolValidator`)
La aplicación valida expresiones usando la pila implementada en la librería.

Casos mínimos probados:
- `(a+b) * [c-d]` → **true**
- `([)]` → **false**

---

## 5) Ejecución desde consola (JAR normal)
Después de compilar `stackHandler`, el JAR se genera en:
- `stackHandler/target/stackHandler-1.0.0.jar`

Ejemplos:
~~~bash
cd stackHandler
java -jar target/stackHandler-1.0.0.jar "(a+b) * [c-d]"
java -jar target/stackHandler-1.0.0.jar "([)]"
~~~

---

## 6) Parte C – Ofuscación reproducible (ProGuard)

### 6.1 Configuración
En `stackHandler` se configuró ProGuard de forma **reproducible desde Maven** mediante un perfil:

- Archivo de reglas: `stackHandler/proguard.pro`
- Perfil Maven: `<id>obfuscate</id>` en `stackHandler/pom.xml`

### 6.2 Comando reproducible
~~~bash
cd stackHandler
mvn clean package -Pobfuscate
~~~

Salida esperada en `stackHandler/target/`:
- `stackHandler-1.0.0.jar` (normal)
- `stackHandler-1.0.0-obfuscated.jar` (ofuscado)

---

## 7) Parte E – Prueba de regresión desde consola (JAR ofuscado)
~~~bash
cd stackHandler
java -jar target/stackHandler-1.0.0-obfuscated.jar "(a+b) * [c-d]"
java -jar target/stackHandler-1.0.0-obfuscated.jar "([)]"
~~~

Resultado: el comportamiento **no cambia** respecto al JAR normal (mismos `true/false`).

---

## 8) Parte D – Ingeniería inversa (decompilación del JAR ofuscado)

### 8.1 Herramienta usada
Se utilizó una herramienta de decompilación (por ejemplo **JD-GUI**) para inspeccionar:

- `stackHandler-1.0.0.jar` (normal)
- `stackHandler-1.0.0-obfuscated.jar` (ofuscado)

### 8.2 Observaciones
- En el JAR **normal** se observan nombres legibles (`LinkedStack`, `Node`, `SymbolValidator`, `App`, etc.).
- En el JAR **ofuscado** los nombres se reemplazan por identificadores genéricos (por ejemplo `a`, `b`, etc.).
- La estructura general es reconocible, pero se pierde claridad semántica y cuesta más seguir la lógica.
- Aun así, con análisis del cuerpo de los métodos se puede inferir el comportamiento (ej. `push/pop`), aunque toma más tiempo.

---

## 9) Evidencias
Las capturas requeridas están en la carpeta `evidencias/`, incluyendo:

- Compilación por consola 
- Listado de JARs (normal vs ofuscado)
- Ejecución por consola (`java -jar ...`) antes y después de ofuscación
- Decompilación del JAR normal vs ofuscado mostrando nombres alterados

---

## 10) Conclusión
Se implementó una pila manual como librería Maven instalada localmente, se consumió desde un segundo proyecto para validar expresiones algebraicas, se generaron JARs ejecutables desde consola y se configuró ofuscación reproducible con ProGuard mediante perfil Maven. La ingeniería inversa demuestra que la ofuscación reduce la legibilidad, sin alterar el comportamiento funcional del programa.

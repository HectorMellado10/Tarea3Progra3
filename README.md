# Tarea 3 – Progra 3 (Stack + StackHandler + Maven + ProGuard)


## Objetivo
Implementar y empaquetar:
1) Una **pila enlazada (LinkedStack)** como librería (`umg.edu.gt.data-structure.stack`)
2) Un **programa cliente (stackHandler)** que usa la pila para validar símbolos/expresiones (paréntesis, corchetes, etc.)
3) **Ofuscar** el JAR final con **ProGuard**, manteniendo la clase principal ejecutable.

---

## Estructura del repositorio
- `umg.edu.gt.data-structure.stack/`  
  Librería con la estructura de datos (por ejemplo: `LinkedStack.java`, `Node.java`).
- `stackHandler/`  
  Aplicación que depende de la librería (por ejemplo: `App.java`, `SymbolValidator.java`).
- `evidencias/`  
  Capturas de compilación, ejecución y análisis (JAR normal vs JAR ofuscado).

---

## Requisitos usados
- **Java (JDK):** 11 (en mi caso: 11.0.29)
- **Maven:** 3.9.12

> Nota: Si el curso pide otra versión (ej. Java 17), el proyecto puede ajustarse, pero en esta entrega se compiló correctamente con Java 11 y Maven 3.9.12.

---

## Compilación y empaquetado

### 1) Librería (Stack)
Desde la carpeta:
```bash
cd umg.edu.gt.data-structure.stack
mvn clean install

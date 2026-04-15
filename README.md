# Bases-De-Datos-Taller-9-1

# Diseño lógico de bases de datos

El diseño lógico permite definir cómo se va a organizar y almacenar la información
antes de implementarla físicamente. Para lograrlo, se apoya en modelos de datos
que describen la estructura de forma precisa y sin ambigüedades.

## Modelos de bases de datos

Un modelo de base de datos define la estructura lógica con la que se representan
los datos y sus vínculos. El modelo utilizado aquí es el **modelo relacional**:

- Organiza la información en tablas llamadas **"relaciones"**.
- Permite conectar datos entre distintas tablas mediante claves.
- Establece cómo los atributos identificadores se convierten en **clave primaria (CP)**
  dentro del esquema lógico.

---

Al aplicar el diseño lógico se obtienen las tablas que resuelven el problema,
siguiendo estas reglas de transformación:

- **Relación N:M (Muchos a Muchos):** Se representa con una **tabla intermediaria**.
- **Relación 1:N (Uno a Muchos):** La clave foránea se ubica en la **tabla del lado N**.

---

# Resolución de Problemas: Diseño de Bases de Datos

## 1. Problema de Datos

Se necesita construir una base de datos para gestionar el préstamo de libros en una
biblioteca universitaria. De cada libro se quiere registrar su título, autor, año de
publicación, número ISBN y el número de copias disponibles. Cada libro pertenece a
una categoría temática, la cual tiene un nombre y una descripción general. Los
estudiantes pueden solicitar préstamos de libros; de cada estudiante se guarda su
nombre, código universitario y correo institucional. Se requiere conocer qué libros
ha tomado prestado cada estudiante, registrando la fecha de préstamo, la fecha de
devolución esperada y si fue devuelto a tiempo.

---

## 2. Diagrama Entidad - Relación (DER)
Entidades y atributos:

- **CATEGORIA:** nombre_cat, descripcion
- **LIBRO:** isbn, titulo, autor, anio, copias_disponibles
- **ESTUDIANTE:** codigo, nombre, correo

---

## 3. Lógica de Transformación de Relaciones

### Relación N:M (Muchos a Muchos) → Creación de una tabla intermediaria

Un estudiante puede tener varios libros prestados, y un libro puede haber sido
prestado por varios estudiantes. Esta relación se resuelve con la tabla **PRESTAMO**.

**PRESTAMO** (isbn_libro, codigo_estudiante, fecha_prestamo, fecha_devolucion, devuelto_a_tiempo)

- **CP:** isbn_libro, codigo_estudiante
- **CAj:** isbn_libro → **LIBRO** (isbn)
- **CAj:** codigo_estudiante → **ESTUDIANTE** (codigo)

---

### Relación 1:N (Uno a Muchos) → La clave va al lado N

Una categoría agrupa muchos libros, pero cada libro pertenece a una sola categoría.

Tablas base:

**CATEGORIA** (nombre_cat, descripcion)
- **CP:** nombre_cat

**LIBRO** (isbn, titulo, autor, anio, copias_disponibles)
- **CP:** isbn

**ESTUDIANTE** (codigo, nombre, correo)
- **CP:** codigo

Después de aplicar la regla 1:N:

**LIBRO** (isbn, titulo, autor, anio, copias_disponibles, nombre_cat)
- **CP:** isbn
- **CAj:** nombre_cat → CATEGORIA (nombre_cat)

---

### Esquema relacional final

| Tabla        | Clave Primaria (CP)                      | Clave Ajena (CAj)                                         |
|--------------|------------------------------------------|-----------------------------------------------------------|
| CATEGORIA    | nombre_cat                               | —                                                         |
| LIBRO        | isbn                                     | nombre_cat → CATEGORIA                                    |
| ESTUDIANTE   | codigo                                   | —                                                         |
| PRESTAMO     | isbn_libro, codigo_estudiante            | isbn_libro → LIBRO / codigo_estudiante → ESTUDIANTE       |

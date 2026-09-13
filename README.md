# Arquitectura MVC modular utilizada en el proyecto

## 1. Definición

El proyecto utiliza una arquitectura basada en **MVC (Model–View–Controller)** implementada sobre CodeIgniter 4, pero siguiendo una convención modular propia de la empresa.

Puede describirse como:

> **MVC modular con lógica de presentación y procesamiento distribuida dentro de los módulos de las vistas.**

No es el MVC académico más común de CodeIgniter, donde el Controller concentra gran parte de la lógica. En esta arquitectura, los Controllers tienen principalmente una función de **enrutamiento**, mientras que cada módulo organiza sus operaciones dentro de sus propias vistas.

---

## 2. Flujo general

El flujo puede representarse así:

```text
                    PETICIÓN DEL USUARIO
                             │
                             ▼
                        CONTROLLER
                       (enrutamiento)
                             │
                             ▼
                           MÓDULO
                             │
          ┌──────────────────┼──────────────────┐
          │                  │                  │
         view              create              edit
          │                  │                  │
          ▼                  ▼                  ▼
       index.php          index.php          index.php
          │                  │                  │
          ▼                  ▼                  ▼
        deny.php           deny.php           deny.php
        form.php           form.php           form.php
     validator.php      validator.php      validator.php
     procesor.php       procesor.php       procesor.php
          │                  │                  │
          └──────────────────┼──────────────────┘
                             │
                             ▼
                           MODEL
                             │
                             ▼
                       BASE DE DATOS
```

---

## 3. Responsabilidad del Controller

El Controller funciona principalmente como punto de entrada y enrutamiento.

Su responsabilidad no es concentrar toda la lógica de negocio.

Por ejemplo:

```text
/products/view
/products/create
/products/edit
/products/delete
/products/list
```

El Controller dirige la petición hacia el módulo correspondiente.

---

## 4. Organización de cada módulo

Cada funcionalidad se divide en operaciones.

Por ejemplo, el módulo `productos` contiene:

```text
productos/
│
├── view/
│   ├── deny.php
│   ├── index.php
│   ├── validator.php
│   ├── form.php
│   └── procesor.php
│
├── create/
│   ├── deny.php
│   ├── index.php
│   ├── validator.php
│   ├── form.php
│   └── procesor.php
│
├── edit/
│   ├── deny.php
│   ├── index.php
│   ├── validator.php
│   ├── form.php
│   └── procesor.php
│
├── delete/
│   ├── deny.php
│   ├── index.php
│   ├── validator.php
│   ├── form.php
│   └── procesor.php
│
└── list/
    ├── deny.php
    ├── index.php
    ├── validator.php
    ├── form.php
    └── procesor.php
```

Cada directorio representa una operación específica.

---

## 5. index.php

El `index.php` funciona como punto de entrada de la operación.

Una de sus responsabilidades principales es determinar si el usuario puede realizar la acción.

Conceptualmente:

```text
index.php
    │
    ▼
¿Tiene permiso?
    │
 ┌──┴──┐
 NO    SÍ
 │      │
 ▼      ▼
deny   form
```

Esto permite que cada operación tenga su propio control de acceso.

---

## 6. deny.php

`deny.php` representa la respuesta cuando el usuario no tiene permisos para realizar la operación solicitada.

Por ejemplo:

```text
Usuario intenta eliminar producto
             │
             ▼
      ¿Tiene permiso?
             │
             ▼
            NO
             │
             ▼
          deny.php
```

---

## 7. form.php

El nombre `form.php` no necesariamente significa que solamente contenga un formulario HTML.

En esta arquitectura representa principalmente la **interfaz de la operación**.

Por ejemplo:

```text
productos/view/form.php
```

puede contener:

- tarjetas de productos
- imágenes
- buscadores
- filtros
- tablas
- botones
- información detallada
- acciones de editar y eliminar

Mientras que:

```text
productos/create/form.php
```

puede contener el formulario para crear un producto.

Y:

```text
productos/edit/form.php
```

puede contener el formulario para modificarlo.

Por lo tanto, `form.php` es la capa visual/interactiva de la operación.

---

## 8. validator.php

`validator.php` se encarga de las validaciones necesarias antes de ejecutar una operación.

Por ejemplo:

```text
¿El nombre existe?
¿El precio es válido?
¿El stock es válido?
¿El SKU es válido?
¿Los datos cumplen las reglas?
```

La validación evita que información incorrecta llegue al procesamiento o a la base de datos.

---

## 9. procesor.php

`procesor.php` ejecuta la operación correspondiente después de superar las validaciones.

Por ejemplo:

```text
CREATE
UPDATE
DELETE
```

Este archivo puede utilizar el Model para realizar la operación correspondiente sobre la base de datos.

---

## 10. Model

El Model es la capa encargada de comunicarse con la base de datos.

Sus responsabilidades incluyen operaciones como:

```text
SELECT
INSERT
UPDATE
DELETE
```

La vista no necesita conocer directamente cómo se realiza la consulta SQL.

El Model proporciona la información o ejecuta la operación solicitada.

---

## 11. Ejemplo del flujo de visualización de productos

Un ejemplo conceptual sería:

```text
Usuario
   │
   ▼
productos/view/index.php
   │
   ▼
¿Tiene permiso?
   │
 ┌─┴─────┐
NO       SÍ
│         │
▼         ▼
deny.php  form.php
             │
             ▼
           MODEL
             │
             ▼
        BASE DE DATOS
             │
             ▼
          productos
             │
             ▼
          form.php
```

La interfaz puede mostrar los productos obtenidos y proporcionar las acciones disponibles.

---

## 12. Relación con MVC

La arquitectura sigue manteniendo los conceptos fundamentales de MVC:

### Model

Representa la comunicación con los datos.

```text
Model → Base de datos
```

### View

Representa la interfaz y la lógica asociada a cada operación del módulo.

```text
View → Interfaz + validaciones + procesamiento de la operación
```

### Controller

Coordina principalmente la entrada y el enrutamiento.

```text
Controller → dirige la petición
```

La diferencia respecto al MVC tradicional es la distribución de responsabilidades.

En un MVC convencional, muchas responsabilidades de procesamiento se concentran en el Controller. En esta arquitectura empresarial, esas responsabilidades se distribuyen dentro de la estructura del módulo.

---

## 13. Ventaja principal

La principal ventaja de este patrón es que cada operación queda encapsulada.

Por ejemplo:

```text
productos/create/
```

contiene todo lo necesario para crear productos.

```text
productos/edit/
```

contiene todo lo necesario para editar productos.

```text
productos/delete/
```

contiene todo lo necesario para eliminarlos.

Esto facilita localizar rápidamente el código relacionado con una operación concreta.

---

## 14. Layout

El layout global queda separado de los módulos.

Su función es proporcionar elementos comunes como:

```text
Sidebar
Navbar
Estilos
Bootstrap
Estructura HTML
Área de contenido
```

El layout no debe conocer los datos específicos de productos, categorías, movimientos, etc.

Conceptualmente:

```text
                 LAYOUT
                    │
        ┌───────────┴───────────┐
        │                       │
     SIDEBAR                 NAVBAR
        │                       │
        └───────────┬───────────┘
                    │
               CONTENIDO
                    │
                    ▼
              MÓDULO ACTUAL
```

Esto permite reutilizar el mismo layout en todos los módulos de la aplicación.

---

## 15. Resumen

La arquitectura puede resumirse de la siguiente manera:

```text
Controller
    ↓
Enruta la petición

View / Módulo
    ↓
Controla la operación y la interfaz

index.php
    ↓
Punto de entrada y permisos

deny.php
    ↓
Acceso denegado

form.php
    ↓
Interfaz de la operación

validator.php
    ↓
Validación

procesor.php
    ↓
Procesamiento

Model
    ↓
Comunicación con la base de datos

Database
    ↓
Persistencia de información
```

Es una **convención arquitectónica empresarial basada en MVC y adaptada a CodeIgniter 4**, no una estructura obligatoria del framework.

Por eso, al desarrollar nuevos módulos, lo más importante es mantener la misma separación y convención que utiliza el resto del sistema.

---

## 16. Organización por tabla de la base de datos

Una característica adicional de la arquitectura utilizada en el proyecto es que la organización principal del código se realiza **por cada tabla de la base de datos**.

Para cada tabla existe su propio conjunto de:

- Modelo
- Controlador
- Vista

Conceptualmente:

```text
TABLA DE BASE DE DATOS
        │
        ├── MODEL
        │
        ├── CONTROLLER
        │
        └── VIEW
             │
             ├── view/
             ├── create/
             ├── edit/
             ├── delete/
             └── list/
```

Por ejemplo, si existe una tabla `productos`, su estructura se organiza alrededor de esa tabla:

```text
productos
│
├── Controller
├── Model
└── View
    ├── view/
    ├── create/
    ├── edit/
    ├── delete/
    └── list/
```

De esta manera, cada tabla mantiene encapsulados sus elementos de acceso a datos, enrutamiento y presentación.

### Escala de la arquitectura

Esta convención puede generar una estructura de proyecto considerablemente grande cuando la base de datos contiene muchas tablas.

Por ejemplo, tomando como referencia una base de datos hipotética con **500 tablas**:

```text
500 tablas
    │
    ├── 500 modelos
    ├── 500 controladores
    └── 500 vistas
```

Esto representa aproximadamente **1.500 componentes principales** asociados directamente a las tablas.

Si cada tabla organiza además sus vistas mediante cinco operaciones y cada operación mantiene los cinco archivos establecidos por la arquitectura:

```text
view/
├── deny.php
├── index.php
├── validator.php
├── form.php
└── procesor.php
```

entonces cada tabla puede llegar a contener:

```text
5 operaciones × 5 archivos = 25 archivos de vista
```

Para 500 tablas:

```text
500 × 25 = 12.500 archivos de operación
```

Y si además se consideran los modelos, controladores y demás elementos estructurales, la cantidad total de directorios y archivos del proyecto puede crecer considerablemente.

Esta gran cantidad de archivos **no representa necesariamente un error de arquitectura**. Es una consecuencia directa de la convención utilizada: priorizar la separación y encapsulación de cada tabla y de cada operación.

### Objetivo de esta organización

La finalidad principal es que, al trabajar sobre una tabla concreta, sea posible localizar rápidamente todos los elementos relacionados con ella.

Por ejemplo:

```text
productos
│
├── Model
│      └── consultas de productos
│
├── Controller
│      └── rutas de productos
│
└── View
       ├── view      → consultar / mostrar
       ├── create    → crear
       ├── edit      → editar
       ├── delete    → eliminar
       └── list      → listar
```

Esto mantiene la lógica de cada entidad separada de las demás y conserva la misma convención en todo el sistema, incluso cuando el proyecto alcanza una gran cantidad de tablas.

---

## 17. Estructura conceptual completa

Teniendo en cuenta todas las características anteriores, la arquitectura completa puede visualizarse así:

```text
                         BASE DE DATOS
                              │
              ┌───────────────┼───────────────┐
              │               │               │
           TABLA 1         TABLA 2         TABLA N
              │               │               │
        ┌─────┴─────┐   ┌─────┴─────┐   ┌─────┴─────┐
        │            │   │            │   │            │
      MODEL     CONTROLLER       MODEL     CONTROLLER  ...
        │            │             │            │
        │            └─────────────┼────────────┘
        │                          │
        └──────────────┐           │
                       ▼           ▼
                       VIEW / MÓDULO
                              │
             ┌────────────────┼────────────────┐
             │                │                │
           view             create           edit
             │                │                │
           delete            list              │
             │                │                │
             └────────────────┼────────────────┘
                              │
                     index.php
                              │
                       ¿Tiene permiso?
                         /         \
                       NO           SÍ
                       │             │
                   deny.php       form.php
                                     │
                              validator.php
                                     │
                               procesor.php
                                     │
                                   MODEL
                                     │
                               BASE DE DATOS
```

El **layout global** permanece separado de esta estructura y funciona como la plantilla visual común de toda la aplicación:

```text
                         LAYOUT GLOBAL
                              │
                ┌─────────────┴─────────────┐
                │                           │
             SIDEBAR                     NAVBAR
                │                           │
                └─────────────┬─────────────┘
                              │
                         CONTENIDO
                              │
                              ▼
                       VISTA DEL MÓDULO
```

El layout no consulta tablas ni contiene lógica específica de las entidades. Su función es proporcionar la estructura visual común y dejar un área donde cada módulo pueda renderizar su contenido.

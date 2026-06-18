# IPC2P
# Actividad de Laboratorio: Arquitectura Multi-Nivel (N-Tier) y Patrón MVC en .NET

**Curso:** Introducción a la Programación y Computación 2  
**Estudiante:** Luis Ángel Saloj Cuma  
**Carné:** 202503517  
**Fecha:** 17 de junio de 2026

---

# Parte 1. Fundamentación Teórica y Análisis Crítico

## 1. El Tránsito hacia los Sistemas Distribuidos y Multi-Capa

### La Limitación del Monolito Local

En una arquitectura monolítica tradicional todos los componentes del sistema (interfaz gráfica, lógica de negocio y almacenamiento de datos) residen dentro de una única computadora. Aunque este enfoque simplifica el desarrollo inicial, presenta importantes limitaciones cuando el sistema crece.

La principal dificultad es la sincronización de la información. Si varios usuarios necesitan acceder simultáneamente al sistema desde diferentes equipos, resulta complejo mantener una única versión consistente de los datos. Además, cada actualización del software obliga a modificar todas las instalaciones del programa.

Desde el punto de vista de la escalabilidad, un sistema monolítico depende completamente de la capacidad del único equipo donde se ejecuta. Si aumenta el número de usuarios o el volumen de datos, todo el sistema comienza a degradar su rendimiento, ya que no es posible distribuir la carga entre varios servidores.

Las arquitecturas distribuidas solucionan este problema separando físicamente cada responsabilidad, permitiendo que múltiples clientes accedan simultáneamente a un mismo servidor de aplicaciones y a una base de datos centralizada.

---

### Diferencia entre Layers (Capas) y Tiers (Niveles)

Aunque ambos conceptos suelen confundirse, representan aspectos completamente distintos de la arquitectura del software.

| Layers (Capas) | Tiers (Niveles) |
|----------------|-----------------|
| Son una división lógica del código fuente. | Son una división física de la infraestructura. |
| Organizan responsabilidades dentro del programa. | Organizan dónde se ejecutan los componentes. |
| Existen aunque todo el sistema esté instalado en una sola computadora. | Cada nivel puede ejecutarse en un servidor distinto. |
| Facilitan el mantenimiento del código. | Mejoran la seguridad, disponibilidad y escalabilidad. |

En otras palabras, las capas representan cómo está organizado el software, mientras que los niveles representan dónde se ejecuta físicamente cada parte del sistema.

---

### Responsabilidades en una Arquitectura de Tres Niveles

#### Nivel 1: Presentation Tier (Capa de Presentación)

Su responsabilidad consiste en interactuar directamente con el usuario. Recibe las solicitudes, muestra la información y captura los datos ingresados.

Tecnologías comunes:

- HTML
- CSS
- JavaScript
- Razor Pages
- ASP.NET Core MVC Views

No debe contener reglas de negocio ni acceso directo a bases de datos.

---

#### Nivel 2: Application Tier (Capa de Aplicación o Negocio)

Es el núcleo del sistema.

Aquí se implementan todas las reglas del negocio, validaciones, cálculos y procesos internos.

Tecnologías comunes:

- ASP.NET Core
- C#
- APIs REST
- Servicios

Este nivel recibe las solicitudes provenientes de la interfaz y decide cómo procesarlas antes de comunicarse con la base de datos.

---

#### Nivel 3: Data Tier (Capa de Datos)

Su función exclusiva es almacenar y administrar la información.

Aquí se ejecutan operaciones como:

- consultas
- inserciones
- actualizaciones
- eliminación de registros

Tecnologías comunes:

- SQL Server
- PostgreSQL
- MySQL
- Oracle
- MongoDB

Este nivel nunca debe comunicarse directamente con el usuario.

---

### Seguridad Perimetral

Exponer públicamente el puerto de una base de datos representa uno de los errores de seguridad más graves en ingeniería de software.

Si un atacante logra acceder directamente al servidor de base de datos podría:

- Robar información confidencial.
- Modificar registros.
- Eliminar datos.
- Ejecutar ataques automatizados de fuerza bruta.
- Comprometer completamente el sistema.

La buena práctica consiste en que únicamente el servidor de aplicaciones tenga acceso a la base de datos mediante una red privada.

El usuario jamás debe comunicarse directamente con el servidor de datos; todas las solicitudes deben pasar primero por la capa de negocio, donde se realizan autenticaciones, autorizaciones y validaciones.

---

# 2. Desacoplamiento Lógico con el Patrón MVC

## La Crisis del Código Espagueti

Cuando un mismo archivo contiene código HTML, consultas SQL, operaciones matemáticas y lógica del negocio, el software se vuelve extremadamente difícil de mantener.

Entre las consecuencias principales se encuentran:

- Baja legibilidad.
- Alta probabilidad de errores.
- Dificultad para agregar nuevas funcionalidades.
- Reutilización casi imposible del código.
- Pruebas unitarias complicadas.
- Mayor tiempo de mantenimiento.

Este problema recibe el nombre de "Código Espagueti", debido a que todas las responsabilidades se encuentran mezcladas.

---

## Separación de Preocupaciones (Separation of Concerns)

El patrón MVC fue propuesto por Trygve Reenskaug para dividir claramente las responsabilidades del software.

### Modelo (Model)

El Modelo representa la información del sistema y las reglas del negocio.

Su función consiste en almacenar los datos y mantener su integridad.

El Modelo nunca debe conocer cómo serán mostrados esos datos al usuario.

Esto permite reutilizar el mismo modelo en diferentes interfaces (web, móvil o escritorio).

---

### Vista (View)

La Vista representa únicamente la interfaz gráfica.

Se considera una entidad pasiva porque únicamente muestra la información que recibe del controlador.

Está estrictamente prohibido que una Vista contenga:

- consultas SQL;
- reglas del negocio;
- algoritmos matemáticos;
- acceso a archivos;
- conexiones a bases de datos.

Su única responsabilidad consiste en presentar información.

---

### Controlador (Controller)

El Controlador funciona como intermediario entre la Vista y el Modelo.

Recibe las solicitudes HTTP provenientes del navegador.

Posteriormente:

1. valida la solicitud;
2. consulta el modelo;
3. procesa la información necesaria;
4. selecciona la vista adecuada;
5. devuelve la respuesta al usuario.

Por esta razón suele denominarse el "director de orquesta" del patrón MVC.

---

## Alta Cohesión y Bajo Acoplamiento

El patrón MVC favorece dos principios fundamentales de la ingeniería de software.

### Alta Cohesión

Cada componente realiza una única responsabilidad específica.

Por ejemplo:

- El Modelo administra datos.
- La Vista presenta información.
- El Controlador coordina las operaciones.

Esto facilita la comprensión y mantenimiento del sistema.

---

### Bajo Acoplamiento

Los componentes dependen lo menos posible unos de otros.

Gracias a ello es posible:

- modificar la interfaz sin alterar el modelo;
- cambiar la base de datos sin afectar la vista;
- realizar pruebas unitarias de manera independiente;
- reutilizar componentes en otros proyectos.

Estas características hacen que el software sea más flexible, escalable y mantenible.

---

# Parte 2. Modelado del Ciclo de Vida y Enrutamiento Semántico

## 1. Tabla de Mapeo de URLs

| URL Entrante | Controlador | Acción | id |
|--------------|-------------|---------|----|
| https://ingenieria.usac.edu.gt/ControlAcademico/Login | ControlAcademicoController | Login | Ninguno |
| https://ingenieria.usac.edu.gt/Estudiante/Historial/20260123 | EstudianteController | Historial | 20260123 |
| https://ingenieria.usac.edu.gt/Asignacion/Detalle/10 | AsignacionController | Detalle | 10 |
| https://ingenieria.usac.edu.gt/Home | HomeController | Index | Ninguno |

---

## 2. Flujo Completo de una Petición HTTP en MVC

### Paso 1

El usuario realiza una acción desde su navegador, por ejemplo haciendo clic sobre un botón o un enlace. El navegador genera una petición HTTP hacia el servidor web.

### Paso 2

El sistema de enrutamiento de ASP.NET Core analiza la URL recibida y determina qué controlador y qué acción deben ejecutarse.

### Paso 3

El controlador recibe la solicitud, valida la información recibida y solicita al Modelo los datos necesarios para responder.

### Paso 4

El Modelo obtiene la información desde la base de datos o desde el almacenamiento correspondiente y devuelve los resultados al controlador.

### Paso 5

El controlador envía los datos a la Vista. La Vista genera dinámicamente el documento HTML, el servidor responde al navegador y finalmente el usuario visualiza la página completamente renderizada.

---

# Conclusiones

La arquitectura N-Tier permite distribuir físicamente los componentes del sistema, incrementando la seguridad, disponibilidad y escalabilidad de las aplicaciones modernas.

El patrón MVC complementa esta arquitectura separando claramente las responsabilidades del código, evitando el acoplamiento excesivo y facilitando el mantenimiento del software.

La combinación de ambas arquitecturas constituye actualmente una de las mejores prácticas para el desarrollo profesional de aplicaciones empresariales utilizando ASP.NET Core.

---

# Referencias Bibliográficas

Facultad de Ingeniería, Universidad de San Carlos de Guatemala. (2026). *Sesión 11: Modelado Base y Arquitecturas de Despliegue. Evolución de Sistemas Distribuidos, Fundamentos del Modelo Cliente-Servidor y Diseño Físico Multi-Capas (N-Tier).* Laboratorio del curso Introducción a la Programación y Computación 2. Guatemala.

Facultad de Ingeniería, Universidad de San Carlos de Guatemala. (2026). *Sesión 12: Arquitectura y Componentes del Patrón MVC. Desacoplamiento Lógico de Software, Ciclo de Vida de las Peticiones y Enrutamiento en Aplicaciones Interactivas Modernas.* Laboratorio del curso Introducción a la Programación y Computación 2. Guatemala.

Microsoft. (2025). *ASP.NET Core MVC Overview.* https://learn.microsoft.com/aspnet/core/mvc/

Microsoft. (2025). *Routing in ASP.NET Core.* https://learn.microsoft.com/aspnet/core/fundamentals/routing/

Martin, R. C. (2018). *Clean Architecture: A Craftsman's Guide to Software Structure and Design.* Pearson.

Gamma, E., Helm, R., Johnson, R., & Vlissides, J. (1994). *Design Patterns: Elements of Reusable Object-Oriented Software.* Addison-Wesley.
# Modulo-Reservas-Espacios
📚 DOCUMENTACIÓN – MÓDULO DE SEDES Y ESPACIOS

Autor: Diego Heredia
Fecha: Diciembre 2025
Tecnología: Spring Boot 3.5.8 + Java 17

📖 ÍNDICE

Resumen Ejecutivo

Arquitectura

Patrones de Diseño

Componentes Implementados

Endpoints REST

Disponibilidad de Espacios

Validaciones

Instalación y Uso

1. RESUMEN EJECUTIVO <a id="resumen"></a>
1.1 Alcance del Módulo

Este módulo implementa la funcionalidad de:

✅ Gestión de Sedes

✅ Gestión de Espacios de Estacionamiento

✅ Estados en tiempo real (DISPONIBLE, OCUPADO)

✅ Creación masiva de espacios

✅ Consulta de disponibilidad

✅ Filtrado dinámico mediante patrones de diseño

Los datos se manejan en tiempo de ejecución, sin base de datos, para simplificar el proyecto académico.

1.2 Métricas del Proyecto
Métrica	Cantidad
Endpoints REST	12
Modelos	3
Services	2
Controllers	2
Patrones de Diseño	2
2. ARQUITECTURA <a id="arquitectura"></a>
2.1 Arquitectura en Capas
┌─────────────────────────────────────┐
│         CAPA PRESENTACIÓN           │
│   (Controllers - REST API)          │
├─────────────────────────────────────┤
│         CAPA NEGOCIO                │
│   (Services + Strategy + Factory)   │
├─────────────────────────────────────┤
│         CAPA DOMINIO                │
│   (Models)                          │
└─────────────────────────────────────┘

2.2 Estructura de Paquetes
com.reservas.estacionamiento/
├── controller/
│   ├── SedeController.java
│   └── EspacioController.java
├── service/
│   ├── SedeService.java
│   └── EspacioService.java
├── model/
│   ├── Sede.java
│   ├── Espacio.java
│   ├── TipoEspacio.java
│   └── EstadoEspacio.java
├── factory/
│   ├── EspacioFactory.java
│   ├── AutoEspacioFactory.java
│   ├── MotoEspacioFactory.java
│   ├── DiscapacitadoEspacioFactory.java
│   ├── VIPEspacioFactory.java
│   └── EspacioFactoryProvider.java
└── strategy/
    ├── DisponibilidadStrategy.java
    ├── DisponibilidadPorTipo.java
    ├── DisponibilidadPorEstado.java
    └── DisponibilidadTotal.java

3. PATRONES DE DISEÑO <a id="patrones"></a>
3.1 Factory Method ⭐

Propósito:
Encapsular la creación de objetos Espacio según su tipo.

Implementación:

EspacioFactory define el método crearEspacio

Cada tipo de espacio tiene su propia fábrica concreta

EspacioFactoryProvider selecciona la fábrica correcta según TipoEspacio

Beneficios:

✅ Elimina if / switch repetidos

✅ Centraliza la creación de objetos

✅ Cumple Open/Closed Principle

3.2 Strategy Pattern ⭐

Propósito:
Permitir múltiples criterios de filtrado de espacios de forma dinámica.

Estrategias implementadas:

Disponibilidad por tipo

Disponibilidad por estado

Disponibilidad total (sin filtro)

Beneficios:

✅ Cada filtro es una clase independiente

✅ Fácil combinación de filtros

✅ Código extensible y mantenible

✅ Cumple SRP y OCP de SOLID

4. COMPONENTES IMPLEMENTADOS <a id="componentes"></a>
4.1 Modelos
Sede
public class Sede {
    private int id;
    private String nombre;
    private String direccion;
    private String ciudad;
}

Espacio
public class Espacio {
    private int id;
    private int numero;
    private TipoEspacio tipo;
    private EstadoEspacio estado;
    private int sedeId;
}

4.2 Servicios
SedeService

Crear, listar, actualizar y eliminar sedes

Gestión en memoria

EspacioService

Crear espacios individuales o masivos

Actualizar estado de espacios

Filtrar disponibilidad usando Strategy

5. ENDPOINTS REST <a id="endpoints"></a>
5.1 Sedes
Método	Endpoint	Descripción
GET	/sedes	Listar todas las sedes
POST	/sedes	Crear sede
GET	/sedes/{id}	Obtener sede
PUT	/sedes/{id}	Actualizar sede
DELETE	/sedes/{id}	Eliminar sede
5.2 Espacios
Método	Endpoint	Descripción
GET	/espacios	Listar espacios
POST	/espacios	Crear espacio
GET	/espacios/sede/{id}	Espacios por sede
PUT	/espacios/{id}/estado	Cambiar estado
DELETE	/espacios/{id}	Eliminar espacio
POST	/espacios/crearMuchos/{sedeId}	Crear espacios masivos
6. DISPONIBILIDAD DE ESPACIOS <a id="disponibilidad"></a>
Ejemplo: Filtrar por tipo y disponibilidad
GET /espacios/filtrar/tipo/AUTO


Resultado:
Solo espacios tipo AUTO con estado DISPONIBLE.

El filtrado se logra combinando estrategias, sin lógica condicional en el controlador.

7. VALIDACIONES <a id="validaciones"></a>

ID de sede debe existir

Tipo de espacio válido

Estado válido

No se permite actualizar espacios inexistentes

8. INSTALACIÓN Y USO <a id="instalacion"></a>
8.1 Requisitos

Java 17+

Gradle

Puerto 8080 libre

8.2 Ejecutar
./gradlew bootRun


Servidor disponible en:

http://localhost:8080

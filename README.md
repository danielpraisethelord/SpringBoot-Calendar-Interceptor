# Documentación de la Aplicación

## Descripción General

Esta aplicación es una demostración del uso de interceptores HTTP en Spring Boot para manejar accesos basados en horarios. Los interceptores se utilizan para verificar si una solicitud se realiza dentro del horario de atención definido y responden apropiadamente si se está fuera del horario.

## Componentes Principales

### 1. `AppController`

`AppController` es un controlador REST que maneja un endpoint: `/foo`. Este endpoint responde con un mensaje de bienvenida, la hora actual y un mensaje personalizado que se establece en el interceptor.

- **Endpoint `/foo`**: Devuelve un mensaje de bienvenida, la hora actual y un mensaje sobre el horario de atención.

### 2. `CalendarInterceptor`

`CalendarInterceptor` es un interceptor que implementa la interfaz `HandlerInterceptor`. Este interceptor realiza las siguientes funciones:

- **`preHandle`**: Se ejecuta antes de que el controlador maneje la solicitud.
  - Verifica si la hora actual está dentro del horario de atención definido en las propiedades de configuración.
  - Si está dentro del horario de atención, establece un mensaje en la solicitud y permite que la solicitud continúe.
  - Si está fuera del horario de atención, devuelve una respuesta de error (401) con un mensaje de cierre.

- **`postHandle`**: Este método está sobrescrito pero no se implementa ninguna lógica en él.

### 3. `SpringbootCalendarInterceptorApplication`

`SpringbootCalendarInterceptorApplication` es la clase principal de la aplicación que arranca la aplicación Spring Boot.

### 4. `MvcConfig`

`MvcConfig` es una clase de configuración que implementa la interfaz `WebMvcConfigurer` para personalizar el comportamiento de Spring MVC. En esta clase se registra el `CalendarInterceptor` para interceptar solicitudes al endpoint `/foo`.

## Archivo de Configuración

### `application.properties`

El archivo `application.properties` contiene las siguientes configuraciones:

- `spring.application.name`: Nombre de la aplicación.
- `server.port`: Puerto en el que se ejecuta la aplicación.
- `config.calendar.open`: Hora de apertura del horario de atención.
- `config.calendar.close`: Hora de cierre del horario de atención.

## Funcionamiento de la Aplicación

1. **Inicio de la Aplicación**:
   - La aplicación se inicia mediante la clase `SpringbootCalendarInterceptorApplication`.

2. **Intercepción de Solicitudes**:
   - Cuando se realiza una solicitud a `/foo`, `CalendarInterceptor` intercepta la solicitud.
   - En el método `preHandle`, se verifica la hora actual contra el horario de atención definido en las propiedades de configuración.
   - Si la solicitud se realiza dentro del horario de atención, se establece un mensaje en la solicitud y se permite que la solicitud continúe.
   - Si la solicitud se realiza fuera del horario de atención, se devuelve una respuesta de error (401) con un mensaje que indica que el servicio está cerrado y especifica el horario de atención.

3. **Respuestas del Controlador**:
   - El controlador `AppController` maneja la solicitud a `/foo`, devolviendo un mensaje de bienvenida, la hora actual y el mensaje establecido por el interceptor sobre el horario de atención.

## Propósito de Cada Clase

- **`AppController`**: Provee el endpoint REST que es interceptado por el interceptor para verificar el horario de atención.
- **`CalendarInterceptor`**: Verifica si la solicitud se realiza dentro del horario de atención y maneja las respuestas apropiadamente.
- **`SpringbootCalendarInterceptorApplication`**: Clase principal que arranca la aplicación Spring Boot.
- **`MvcConfig`**: Configura la aplicación Spring MVC y registra el interceptor para que se aplique al endpoint especificado.

Esta estructura permite demostrar cómo los interceptores pueden ser usados para manejar la lógica de acceso basada en horarios en una aplicación Spring Boot.

# WinesApp - API REST para Gestión de Vinos

Esta es una API REST desarrollada en C# utilizando ASP.NET Core. La aplicación permite gestionar vinos, usuarios y autenticación basada en JWT. Está diseñada para manejar operaciones CRUD sobre vinos y usuarios, así como la gestión de autenticación con tokens JWT.

## Índice

1. [Características](#características)
2. [Requisitos](#requisitos)
3. [Instalación](#instalación)
4. [Endpoints](#endpoints)
   - [Autenticación](#autenticación)
   - [Usuarios](#usuarios)
   - [Vinos](#vinos)
5. [Excepciones Personalizadas](#excepciones-personalizadas)
6. [Autenticación](#autenticación-1)

## Características

- Autenticación de usuarios mediante JWT (JSON Web Tokens).
- Gestión de usuarios (creación).
- Gestión de vinos (consultar, agregar, actualizar stock, consultar por variedad, entre otros).
- Verificación y manejo de excepciones personalizadas para variedades y regiones de vino.

## Requisitos

- .NET 6.0 o superior
- ASP.NET Core
- Entity Framework Core (con SQLite)
- Autenticación JWT

## Instalación

1. Clonar el repositorio:
    ```bash
    git clone https://github.com/tu_usuario/winesapp.git
    ```

2. Navegar al directorio del proyecto:
    ```bash
    cd winesapp
    ```

3. Configurar la cadena de conexión de la base de datos en el archivo `appsettings.json`:
    ```json
    {
        "ConnectionStrings": {
            "WineAppAPIDBConnectionString": "Data Source=winesapp.db"
        },
        "Authentication": {
            "SecretForKey": "clave_secreta_para_jwt",
            "Issuer": "tu_emisor",
            "Audience": "tu_audiencia"
        }
    }
    ```

4. Restaurar paquetes NuGet:
    ```bash
    dotnet restore
    ```

5. Ejecutar las migraciones de la base de datos:
    ```bash
    dotnet ef database update
    ```

6. Ejecutar la aplicación:
    ```bash
    dotnet run
    ```

## Endpoints

### Autenticación

- **POST /api/authentication/authenticate**  
  Autentica un usuario y genera un token JWT.

    **Body**:
    ```json
    {
        "username": "string",
        "password": "string"
    }
    ```

    **Respuestas**:
    - `200 OK`: Devuelve el token JWT.
    - `401 Unauthorized`: Credenciales incorrectas.

### Usuarios

- **POST /api/user**  
  Crea un nuevo usuario.

    **Body**:
    ```json
    {
        "username": "string",
        "password": "string"
    }
    ```

    **Respuestas**:
    - `201 Created`: Usuario creado con éxito.
    - `400 Bad Request`: Cuerpo de la solicitud no válido.

### Vinos

- **GET /api/wine**  
  Obtiene todos los vinos.

    **Respuestas**:
    - `200 OK`: Devuelve la lista de vinos.

- **GET /api/wine/{id}**  
  Obtiene un vino por su ID.

    **Respuestas**:
    - `200 OK`: Devuelve el vino.
    - `404 Not Found`: Vino no encontrado.

- **POST /api/wine**  
  Agrega un nuevo vino.

    **Body**:
    ```json
    {
        "name": "string",
        "variety": "string",
        "year": 2020,
        "region": "string",
        "stock": 0
    }
    ```

    **Respuestas**:
    - `201 Created`: Vino creado con éxito.
    - `400 Bad Request`: Datos de vino inválidos.
    - `409 Conflict`: El vino ya existe.

- **GET /api/wine/variety/{variety}**  
  Obtiene vinos por su variedad.

    **Respuestas**:
    - `200 OK`: Devuelve los vinos.
    - `400 Bad Request`: Variedad de vino inválida.

- **PUT /api/wine/update-stock/{wineId}**  
  Actualiza el stock de un vino.

    **Body**:
    ```json
    100
    ```

    **Respuestas**:
    - `204 No Content`: Stock actualizado.
    - `404 Not Found`: Vino no encontrado.

## Excepciones Personalizadas

La aplicación maneja varias excepciones personalizadas para garantizar la integridad de los datos:

- `InvalidWineVarietyException`: Se lanza cuando la variedad de vino es inválida.
- `InvalidWineRegionException`: Se lanza cuando la región de vino es inválida.
- `WineAlreadyExistsException`: Se lanza cuando un vino ya existe en el sistema.
- `NotFoundException`: Se lanza cuando no se encuentra un recurso solicitado.

## Autenticación

La autenticación se realiza mediante JWT. El cliente debe incluir el token JWT en el encabezado de autorización de la siguiente manera:


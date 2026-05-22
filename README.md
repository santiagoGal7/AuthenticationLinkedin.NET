# AuthenticationLinkedin.NET

Aplicación backend para autenticación con LinkedIn, Google y Microsoft, diseñada para usarse con un frontend separado.

## Requisitos

- .NET 10 SDK
- PostgreSQL ejecutando en `localhost:5432`
- Un frontend compatible ((https://github.com/santiagoGal7/AuthenticationLinkedin.NET))
## Web publica
https://unchainable-keri-semiexternally.ngrok-free.dev/
## Estructura principal

- `Api/`: proyecto backend ASP.NET Core
- `Application/`: lógica de aplicación y casos de uso
- `Domain/`: entidades de dominio y enums
- `Infrastructure/`: contexto de datos, repositorios y servicios

## Uso local

1. Instala .NET 10 SDK.
2. Asegúrate de que PostgreSQL está disponible en `localhost:5432`.
3. Configura las credenciales necesarias.
4. Desde la carpeta raíz del repositorio, ejecuta:

```powershell
cd Api
dotnet run
```

La API quedará disponible en `http://localhost:5152`.

## Configuración importante

### Base de datos

El proyecto usa la cadena de conexión en `Api/appsettings.json`:

```json
"ConnectionStrings": {
  "Postgres": "Host=localhost;Port=5432;Database=linkedinDb;Username=postgres;Password=...;Include Error Detail=true"
}
```

Reemplaza el valor de `Password` con la contraseña de tu instancia de PostgreSQL o define la variable de entorno `POSTGRES_PASSWORD` antes de ejecutar la API.

### JWT

En `Api/appsettings.json` se define la sección `JWT`. Cambia `Key`, `Issuer` y `Audience` por valores propios si usas el proyecto más allá de pruebas locales.

### OAuth

En `Api/appsettings.json` hay valores de configuración para Google y Microsoft:

```json
"Authentication": {
  "Google": {
    "ClientId": "..."
  },
  "Microsoft": {
    "ClientId": "REPLACE_WITH_MICROSOFT_CLIENT_ID",
    "TenantId": "common"
  }
}
```

- Reemplaza `Authentication:Google:ClientId` con tu ID de cliente de Google.
- Reemplaza `Authentication:Microsoft:ClientId` con tu ID de cliente de Microsoft.
- Mantén `TenantId` como `common` si quieres admitir cualquier inquilino de Azure AD.

### Email

El sistema de verificación por email puede funcionar usando SMTP o Resend.

En `Api/appsettings.json` hay una sección `Email` con dos opciones:

- `Provider`: `Auto`, `Smtp` o `Resend`
- `Smtp`: información del servidor SMTP
- `Resend`: clave de API y remitente

Para usar SMTP con Gmail, configura al menos estas variables de entorno antes de ejecutar la API:

```powershell
$env:Email__Smtp__UserName = "tu-email@gmail.com"
$env:Email__Smtp__Password = "tu_contraseña_de_aplicación"
$env:Email__Smtp__FromEmail = "tu-email@gmail.com"
```

Si no configuras un proveedor de correo real, el sistema de desarrollo registra los códigos de verificación en `Api/logs/verification-emails.log`.

### Orígenes permitidos (CORS)

La configuración en `Api/appsettings.json` define los orígenes permitidos:

- `http://localhost:3000`
- `http://localhost:3001`
- `http://localhost:4173`
- `http://127.0.0.1:3000`
- `http://127.0.0.1:4173`
- `http://localhost:5173`

Asegúrate de que el frontend use uno de esos orígenes o añade el origen adecuado aquí.

## Valores que debes reemplazar

- `Api/appsettings.json` -> `ConnectionStrings:Postgres` (usuario/contraseña, nombre de base de datos)
- `Api/appsettings.json` -> `JWT:Key`, `JWT:Issuer`, `JWT:Audience`
- `Api/appsettings.json` -> `Authentication:Google:ClientId`
- `Api/appsettings.json` -> `Authentication:Microsoft:ClientId`
- `Api/appsettings.json` -> `Email:Smtp:UserName`, `Email:Smtp:Password`, `Email:Smtp:FromEmail`
- `Api/appsettings.json` -> `Email:Resend:ApiKey`, `Email:Resend:FromEmail` (si usas Resend)

## Frontend

El frontend compatible está disponible en:

https://github.com/santiagoGal7/AuthenticationLinkedin.NET

## Notas adicionales

- La API crea la base de datos si no existe y aplica migraciones al iniciar.
- Usa `appsettings.Development.json` para valores de desarrollo, pero no dejes credenciales reales en el repositorio.
- Si necesitas conectar el frontend, configura el `ClientId` de Google tanto en el backend como en el frontend correspondiente.

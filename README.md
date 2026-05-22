# 🔐 AuthenticationLinkedin.NET — Backend API

API REST en **.NET 10** que implementa autenticación completa al estilo LinkedIn: registro local, login con email/contraseña, OAuth con Google y Microsoft, verificación de email por código y flujo de onboarding.

## 🔗 Repositorios del proyecto

| Parte | Repositorio |
|---|---|
| 🖥️ Frontend principal | [FrontendAuthenticationLinkedin](https://github.com/Kevinsierra1/FrontendAuthenticationLinkedin.git) |
| 📁 Práctica | [Practica](https://github.com/Amarillo2410/Practica.git) |

---

## ✨ Características principales

- Registro y login local con email/contraseña (hash PBKDF2)
- Login con **Google** y **Microsoft** (OAuth 2.0 via ID Token)
- Verificación de email por código de **6 dígitos**
- Tokens **JWT + Refresh Token**
- Subida de foto de perfil (máx. 5 MB)
- Flujo de **onboarding por pasos**

---

## 🛠️ Tecnologías

- **.NET 10** / ASP.NET Core
- **PostgreSQL** + Entity Framework Core 10
- **MediatR** (CQRS) + **FluentValidation**
- **JWT**, Google.Apis.Auth, Microsoft OpenIdConnect
- Email: SMTP (Gmail), Resend API o log local
- **Swagger / OpenAPI**

---

## 🚀 Requisitos y arranque

**Requisitos:** .NET 10 SDK · PostgreSQL en `localhost:5432`

```bash
# Bash
export POSTGRES_PASSWORD="tu_password"
cd Api && dotnet run

# PowerShell
$env:POSTGRES_PASSWORD = "tu_password"
cd Api; dotnet run
```

La API queda en **http://localhost:5152** — Swagger en **http://localhost:5152/swagger**

> La base de datos y las migraciones se aplican automáticamente al arrancar.

---

## ⚙️ Variables de entorno opcionales

| Variable | Uso |
|---|---|
| `Email__Smtp__UserName` | Cuenta Gmail remitente |
| `Email__Smtp__Password` | Contraseña de aplicación de Gmail |
| `Email__Smtp__FromEmail` | Dirección de origen del email |

> Sin SMTP configurado, el código de verificación se guarda en `Api/logs/verification-emails.log` y se muestra en consola en desarrollo.

Para **Google OAuth**, el `ClientId` debe coincidir en:
- Backend → `appsettings.json`: `Authentication:Google:ClientId`
- Frontend → `.env`: `VITE_GOOGLE_CLIENT_ID`

---

## 📡 Endpoints

| Método | Ruta | Descripción |
|---|---|---|
| `POST` | `/api/auth/register/local` | Registro con email y contraseña |
| `PUT` | `/api/auth/register/local/profile` | Actualizar perfil (onboarding) |
| `POST` | `/api/auth/register/local/profile-photo` | Subir foto de perfil |
| `DELETE` | `/api/auth/register/cancel` | Cancelar registro incompleto |
| `POST` | `/api/auth/login/local` | Login local |
| `POST` | `/api/auth/external-login/google` | Login con Google |
| `POST` | `/api/auth/external-login/microsoft` | Login con Microsoft |
| `POST` | `/api/auth/email-verification/send` | Enviar código de verificación |
| `POST` | `/api/auth/email-verification/verify` | Verificar código de 6 dígitos |

---

## 📁 Estructura

```
AuthenticationLinkedin.NET/
├── Api/             → Controladores, DTOs, middlewares
├── Application/     → Casos de uso (CQRS), validaciones
├── Domain/          → Entidades y enums
└── Infrastructure/  → EF Core, JWT, OAuth, Email
```

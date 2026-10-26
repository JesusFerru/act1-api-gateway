# act1-api-gateway

Actividad 1 del Módulo 5: API Gateway en .NET 8 con [Ocelot](https://github.com/ThreeMammals/Ocelot) y autenticación JWT (Bearer).

## Rutas

| Endpoint de entrada | ¿Requiere Bearer token? | Acción |
|---|---|---|
| `POST /api/login` | No | Genera un Bearer Token a partir de las credenciales enviadas. |
| `GET /api/users` | Sí | Redirige a `https://jsonplaceholder.typicode.com/users` |
| `GET /api/posts` | Sí | Redirige a `https://jsonplaceholder.typicode.com/posts` |

- El login está en `src/ApiGateway/Program.cs`.
- El ruteo está en `src/ApiGateway/ocelot.json`.
- Sin token, o con un token inválido, `/api/users` y `/api/posts` responden `401`.

## Credenciales (estáticas)

Definidas en `src/ApiGateway/appsettings.json`:

| Usuario | Contraseña |
|---|---|
| `admin` | `admin123` |
| `estudiante` | `diplomado2026` |

El token dura 60 minutos.

## Ejecutar con Docker

```bash
docker compose up --build
```

El gateway queda en `http://localhost:8080`.

## Ejecutar sin Docker

Requiere el SDK de .NET 8.

```bash
dotnet run --project src/ApiGateway
```

## Probar

1. Obtener el token:

```bash
curl -X POST http://localhost:8080/api/login -H "Content-Type: application/json" -d '{"username":"admin","password":"admin123"}'
```

Respuesta:

```json
{ "access_token": "eyJhbGciOi...", "token_type": "Bearer", "expires_at": "..." }
```

2. Llamar a las rutas protegidas con el token:

```bash
curl http://localhost:8080/api/users -H "Authorization: Bearer <access_token>"
curl http://localhost:8080/api/posts -H "Authorization: Bearer <access_token>"
```

Credenciales incorrectas en el login devuelven `401`.

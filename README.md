# Fidelización — App Android

Espacio reservado para la aplicación Android del sistema de fidelización. **Todavía
no contiene código de aplicación**: este repositorio se creó para separar el
proyecto y evitar mezclar el desarrollo móvil con la web y el API.

> **Repositorio:** `https://github.com/leoCortes123/fidelizacionApp`
>
> Repositorios relacionados:
> [`fidelizacionWeb`](https://github.com/leoCortes123/fidelizacionWeb) ·
> [`fidelizacionApi`](https://github.com/leoCortes123/fidelizacionApi)

## Objetivo

App para clientes del programa de fidelización:

- iniciar sesión con Google o correo/contraseña (Supabase Auth);
- consultar saldo de puntos y recompensas disponibles;
- ver historial de compras y canjes;
- escanear el QR de una compra para reclamar los puntos;
- recibir el estado del canje aplicado por caja.

El panel de administración seguirá siendo web; la app se enfoca en el cliente.

## Stack propuesto

| Capa | Tecnología |
| --- | --- |
| Lenguaje | Kotlin |
| UI | Jetpack Compose + Material 3 |
| Arquitectura | MVVM + repositorios, estado unidireccional |
| Navegación | Navigation Compose |
| Backend | Supabase (Auth, PostgREST, RPC) del repo `fidelizacionApi` |
| Cliente Supabase | `supabase-kt` (o REST/RPC con Ktor si se prefiere) |
| Cámara/QR | CameraX + ML Kit Barcode Scanning |
| Pruebas | JUnit + Compose UI Test; E2E manual contra staging |

> La decisión de `supabase-kt` vs. un cliente HTTP propio se tomará al crear el
> proyecto. La app **solo** usará la clave pública (`anon` / `publishable`).

## Estructura prevista

```text
app/
  src/main/java/<paquete>/
    data/          Supabase, DTOs y repositorios
    domain/        modelos y casos de uso
    ui/            pantallas Compose, tema y navegación
  src/main/res/    recursos
  src/test/        pruebas unitarias
  src/androidTest/ pruebas de UI
gradle/            catálogo de versiones (libs.versions.toml)
docs/              notas específicas de la app (opcional)
```

## Primeros pasos

1. Crear el proyecto en Android Studio con **Empty Activity (Jetpack Compose)**.
2. Paquete sugerido: `com.fidelizacion.app` (ajustar a la marca definitiva).
3. `minSdk` 26+, `targetSdk` estable actual, Kotlin y AGP actuales.
4. Configurar `SUPABASE_URL` y `SUPABASE_ANON_KEY` en `local.properties` o en
   variables de entorno de Gradle; **no** en `BuildConfig` versionado con valores
   reales.
5. Implementar login, sesión persistente y consulta de saldo antes de añadir el
   escáner QR.

## Configuración y secretos

```dotenv
SUPABASE_URL=
SUPABASE_ANON_KEY=
```

- `SUPABASE_ANON_KEY` es pública por diseño; la seguridad real está en RLS y RPC.
- Nunca incluir `service_role`, `sb_secret_…`, keystores ni `google-services.json`
  en el repositorio.
- `.env`, `local.properties`, `*.jks` y `*.keystore` están en `.gitignore`.

## Documentación

La documentación completa del sistema vive en `documentacion/`, en el workspace
local, al mismo nivel que los repositorios. Allí están la arquitectura, el contrato
del API, las reglas de negocio y el plan de la app (`APP_ANDROID.md`).

## Estado

- [x] Repositorio y espacio de trabajo creados.
- [x] Convenciones de secretos y `.gitignore`.
- [ ] Proyecto Android Studio.
- [ ] Autenticación y sesión.
- [ ] Saldo, recompensas e historial.
- [ ] Escaneo y reclamo de QR.
- [ ] Publicación en Play Store.

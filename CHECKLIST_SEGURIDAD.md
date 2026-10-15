# Checklist de Seguridad — Pontify FrontEnd Mobile

**Proyecto:** Pontify (React Native + Expo)  
**Alcance:** Aplicación móvil (frontend), autenticación con Keycloak y consumo del backend Pontify  
**Fecha de revisión:** 14/10/2026

| Categoría | Criterio | ✔/✘ | Observaciones |
| --- | --- | :---: | --- |
| Autenticación | Requiere usuario y contraseña | ✔ | El inicio de sesión se hace contra **Keycloak** (OpenID Connect) con su formulario de usuario y contraseña. La app nunca recibe ni procesa las credenciales. |
| Autenticación | No permite acceso sin login | ✔ | Las rutas privadas están protegidas con `Stack.Protected guard={isAuthenticated}` (checkout, crear tienda, editar perfil, notificaciones). Carrito, agregar al carrito y punto de encuentro usan `useRequireAuth`, que redirige al login. |
| Autenticación | Flujo de autenticación seguro | ✔ | Se usa *Authorization Code* con **PKCE** (`expo-auth-session`, `usePKCE: true`). El código se intercambia por tokens sin exponer un *client secret* en el dispositivo. |
| Autenticación | Registro gestionado por el proveedor de identidad | ✔ | El registro se delega a Keycloak (`kc_action=register`). La app no crea ni guarda cuentas por su cuenta. |
| Autenticación | Expiración y renovación de sesión | ✔ | Se valida la expiración del *access token* con un margen de tolerancia. Se renueva con el *refresh token* y, ante un `401`, se reintenta una sola vez. Si la renovación falla, se borran los tokens y la sesión queda marcada como expirada. |
| Roles | Existen roles definidos | ✔ | Hay dos perfiles: **Comprador** (usuario autenticado) y **Dueño de tienda** (`store_owner`, rol de Keycloak definido en `AUTH_ROLES`). |
| Roles | Permisos diferenciados | ✔ | La pestaña de gestión de tienda (`Tabs.Protected guard={isStoreOwner}`) y el detalle de pedidos de la tienda (`Stack.Protected`) solo existen para el dueño. La barra de navegación también cambia según el rol. |
| Roles | Roles obtenidos de fuente confiable | ✔ | Los roles se leen de los *claims* del JWT emitido por Keycloak (`jwtService.getRoles`). El cliente no puede asignarse roles. |
| Datos | No muestra datos sensibles | ✔ | Cada usuario consulta solo su información (`/users/me`, `/stores/me`, `/orders`). El backend resuelve al usuario a partir del token. El panel de depuración de sesión muestra los tokens **enmascarados**. |
| Datos | No muestra contraseñas | ✔ | La app nunca maneja contraseñas: la autenticación ocurre por completo en Keycloak. |
| Datos | Almacenamiento seguro de credenciales | ✔ | Los tokens se guardan en **`expo-secure-store`** (Keychain en iOS, Keystore en Android) y no en almacenamiento plano. Al leerlos se validan con un *type guard* (`isAuthTokens`). |
| Datos | Configuración sensible fuera del repositorio | ✔ | El archivo `.env` está en `.gitignore`. Las variables requeridas se validan al iniciar (`env.ts`) y la app falla de forma controlada si falta alguna. |
| Validación | Campos obligatorios validados | ✔ | Los formularios (crear tienda, producto, variante, topping, descuento, perfil) solo arman la petición si los campos requeridos están completos. Si falta alguno, el botón de enviar queda deshabilitado. |
| Validación | Evita datos incorrectos | ✔ | Todos los campos de texto tienen `maxLength`. El teléfono se normaliza a dígitos y se valida (`isValidPhoneNumber`). El porcentaje de descuento tiene límite de dígitos y las fechas se eligen con un selector nativo. |
| Validación | Tipado estricto de datos | ✔ | TypeScript estricto sin `any`, con interfaces para cada petición y respuesta del backend en `models/`. |
| Errores | Errores no muestran info técnica | ✔ | Las pantallas tienen estados de error dedicados (`CartErrorState`, `OrdersErrorState`, `StoreProductsCatalogError`, etc.) con mensajes genéricos. Los errores del logout remoto se silencian. |
| Errores | Mensajes claros y seguros | ✔ | Los mensajes están orientados al usuario, con título y acción sugerida (p. ej. *"No pudimos marcar el pedido como enviado. Intenta de nuevo."*) y botón de reintento en los estados de error. |
| Errores | Fallos no rompen la aplicación | ✔ | Si una petición falla con datos ya cargados, se siguen mostrando esos datos. Las peticiones canceladas (`AbortController`) no se tratan como errores. |
| Sistema | Existe logout | ✔ | Botón de cerrar sesión en Perfil. Ejecuta las limpiezas registradas (`logoutCleanupService`), borra los tokens del dispositivo y cierra la sesión en Keycloak. |
| Sistema | No permite acciones sin permisos | ✔ | Toda petición al backend lleva el header `Authorization: Bearer`. Las acciones de tienda (aceptar, rechazar, despachar y entregar pedidos; editar catálogo) solo son accesibles para el dueño, y el backend valida el token. Además, se bloquean acciones duplicadas mientras hay una en curso. |
| Sistema | Limpieza de sesión al salir | ✔ | Al cerrar sesión se desregistra el token de notificaciones push del dispositivo y se limpia el estado y la caché del usuario. Así, otro usuario no recibe notificaciones ni ve datos ajenos. |

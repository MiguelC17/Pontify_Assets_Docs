# Matriz de Roles y Permisos — Pontify

**Roles:**
- **Comprador:** usuario autenticado sin tienda.
- **Vendedor:** usuario autenticado con el rol `store_owner` de Keycloak. Lo obtiene al crear su tienda y conserva todas las funciones de comprador.

**Leyenda:** `X` = tiene el permiso · vacío = no lo tiene

## Autenticación y cuenta

| Funcionalidad | Vendedor | Comprador |
| --- | :---: | :---: |
| Registrarse | X | X |
| Iniciar sesión (login) | X | X |
| Cerrar sesión (logout) | X | X |
| Ver perfil | X | X |
| Editar perfil (foto y teléfono) | X | X |
| Ver notificaciones | X | X |
| Marcar notificaciones como leídas | X | X |
| Recibir notificaciones push de sus pedidos | X | X |

## Exploración del catálogo

| Funcionalidad | Vendedor | Comprador |
| --- | :---: | :---: |
| Ver inicio (categorías, ofertas, tiendas destacadas) | X | X |
| Buscar productos y tiendas | X | X |
| Filtrar búsqueda por categoría | X | X |
| Ver detalle de una tienda | X | X |
| Ver detalle de un producto y sus reseñas | X | X |
| Seleccionar punto de encuentro | X | X |

## Compra

| Funcionalidad | Vendedor | Comprador |
| --- | :---: | :---: |
| Personalizar producto (variante, toppings, instrucciones) | X | X |
| Agregar productos al carrito | X | X |
| Modificar cantidades / eliminar del carrito | X | X |
| Vaciar carrito | X | X |
| Seleccionar método de pago (transferencia / efectivo) | X | X |
| Confirmar compra (generar pedido) | X | X |
| Ver sus pedidos (en curso e historial) | X | X |
| Cancelar su pedido (mientras está pendiente) | X | X |
| Cambiar la dirección / punto de entrega de su pedido | X | X |
| Calificar pedido entregado (productos y tienda) | X | X |

## Gestión de tienda

| Funcionalidad | Vendedor | Comprador |
| --- | :---: | :---: |
| Crear tienda | | X |
| Ver panel de gestión de su tienda | X | |
| Editar información de la tienda (nombre, logo, banner, contacto) | X | |
| Abrir / cerrar la tienda | X | |
| Ver estadísticas de la tienda (órdenes completadas, calificación) | X | |

## Gestión de catálogo

| Funcionalidad | Vendedor | Comprador |
| --- | :---: | :---: |
| Ver catálogo de su tienda | X | |
| Agregar producto | X | |
| Editar producto | X | |
| Activar / desactivar disponibilidad de producto | X | |
| Crear / eliminar / activar variantes | X | |
| Crear / eliminar / activar toppings | X | |
| Aplicar / reemplazar / quitar descuentos | X | |

## Gestión de pedidos de la tienda

| Funcionalidad | Vendedor | Comprador |
| --- | :---: | :---: |
| Ver pedidos recibidos (pendientes, activos, completados, no completados) | X | |
| Ver resumen de pedidos por estado (KPIs) | X | |
| Ver detalle de un pedido recibido y datos de contacto del cliente | X | |
| Aceptar pedido | X | |
| Rechazar pedido | X | |
| Marcar pedido como enviado | X | |
| Confirmar entrega | X | |

## Notas

- **Crear tienda:** cada usuario puede tener **una sola tienda** (`/stores/me`). Por eso la opción solo aplica al comprador: al crearla pasa a ser vendedor.
- **Confirmar entrega:** la hace el vendedor (`PATCH /orders/{id}/deliver`). El comprador no confirma la entrega, pero puede calificar el pedido una vez entregado.
- **Navegación sin sesión:** el inicio, la búsqueda y los detalles de tienda y producto se pueden ver sin iniciar sesión. Agregar al carrito o comprar exige login.
- **Control de acceso:** en la app, las pantallas de vendedor están protegidas por el rol `store_owner` (`Tabs.Protected` / `Stack.Protected`). En el backend, cada petición se valida con el token de Keycloak.

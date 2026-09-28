# PC Remote

Esta es una aplicación Android sencilla para acceder desde el teléfono a una interfaz web que se está ejecutando en mi PC.

La aplicación no contiene el servidor que controla el PC. Su función es abrir esa interfaz dentro de un `WebView` y mantener la navegación dentro de la aplicación. Para que funcione, el servidor debe estar disponible en:

```text
tu_ip_de_tailscale_del_PC
```

El proyecto también contempla el uso de Tailscale para que el teléfono y el PC puedan comunicarse.

## Cómo funciona

Cuando abro la aplicación, ocurre lo siguiente:

1. Se muestra una pantalla completa, sin barra de estado.
2. Se configura un `WebView` con JavaScript, almacenamiento DOM, cookies y reproducción multimedia sin interacción inicial.
3. Se carga la URL del servidor definida en `MainActivity.java`.
4. Mientras la página carga, aparece una barra de progreso en la parte superior.
5. Las páginas del servidor siguen abiertas dentro de la aplicación.
6. Los enlaces que llevan a otros sitios se abren en el navegador del teléfono.
7. Si no se puede conectar con el PC, aparece un mensaje con las comprobaciones básicas y un botón para reintentar.
8. Si estoy navegando dentro de la interfaz web, el botón de volver regresa a la página anterior en lugar de cerrar la aplicación.

La aplicación también permite seleccionar archivos desde la interfaz web cuando el servidor solicita una subida.

## Requisitos

Para compilar el proyecto necesita:

- Android Studio con soporte para proyectos Gradle.
- JDK 17.
- Android SDK con Android API 35.
- Un dispositivo o emulador con Android 7.0 (API 24) o superior.
- El servidor web funcionando en el PC.
- Tailscale activo tanto en el PC como en el teléfono, si esa es la red utilizada para acceder al servidor.

El repositorio solo contiene la aplicación Android. No incluye el servidor del PC ni una configuración para levantarlo automáticamente.

## Descargar el proyecto

Clono el repositorio con:

```bash
git clone https://github.com/Danisan1804/PC_Remote.git
cd PC_Remote
```

Después abro esa carpeta desde Android Studio y espero a que Gradle termine de sincronizar el proyecto.

## Configuración de la conexión

La dirección del servidor está escrita directamente en `MainActivity.java`:

```java
private static final String SERVER_URL = "IP_Tailscale_PC";
```

Si mi servidor utiliza otra dirección o puerto, cambio ese valor antes de compilar. También debo asegurarme de que el teléfono pueda alcanzar esa dirección y de que el servidor acepte conexiones desde el dispositivo.

Como la aplicación usa una dirección `http`, el manifiesto permite tráfico sin cifrar mediante `usesCleartextTraffic="true"`. Esto está pensado para la conexión actual dentro de la red configurada, pero no sustituye el uso de HTTPS cuando la aplicación se exponga fuera de una red controlada.

## Ejecutar la aplicación

1. Inicio el servidor web en el PC.
2. Compruebo que el PC esté encendido y conectado.
3. Activo Tailscale en el PC y en el teléfono si estoy utilizando esa red.
4. Verifico desde el teléfono que la dirección del servidor responde.
5. Conecto el teléfono por USB o inicio un emulador desde Android Studio.
6. Pulso **Run** en Android Studio y selecciono el dispositivo.

Si la conexión falla, reviso el mensaje que muestra la aplicación, compruebo que el servidor esté corriendo y pulso **Reintentar**.

## Generar el APK

Desde la raíz del proyecto ejecuto:

```bash
./gradlew assembleDebug
```

En Windows puedo ejecutar el mismo comando desde una terminal compatible con scripts `.sh`, o abrir el proyecto en Android Studio y usar la tarea de Gradle `app > Tasks > build > assembleDebug`.

El APK de depuración se genera en:

```text
app/build/outputs/apk/debug/app-debug.apk
```

El proyecto también tiene un flujo de GitHub Actions que se ejecuta al hacer `push` o manualmente. Ese flujo compila el APK de depuración y lo publica como artefacto con el nombre `PCRemote-APK`.

## Estructura principal

```text
ContIA/

# Guía de Mejores Prácticas - Pilbeo (Kotlin Multiplatform)

Este docuimento define las directrices y estándares para el desarrollo del proyecto **Pilbeo**, una
aplicación Kotlin Multiplatform (KMP) que comparte lógica y UI (Compose Multiplatform) entre Android
e IOS.

## 1. General

- Utiliza buenas prácticas en desarrollo de software y aplicaciones móviles Android e iOS.
- Sigue la guía de estilos más actual de Kotlin.
- La app soporta idíoma español e inglés. Siendo inglés el idioma por defecto.
    - Los textos deben estar estructurados por pantalla en su fichero de `strings.xml`.

## 2. Arquitectura

Se sigue el patrón **MVVM (Model-View-ViewModel)** adaptado a KMP:

- **UI (Compose):** Rside en `commonMain`. Debe ser declarativa y reactiva.
- **ViewModel:** Reside en `commonMain` utilizando la librería de Lifecycle de Jetpack compatible con KMP.
- **Repository/UseCases:** Lógica de negocio y acceso a datos en `commonMain`.
- **Platform Specific:** Solo cuando sea estrictamente necesario, utilizar `expect/actual` en `androidMain` o `iosMain`.

## 3. Gestión de UI y Compose

- **Componentes Compartidos: ** Toda la UI debe intentarse contruir en `commonMain` usando Compose Multiplatform.
- **Material 3:** Utilizar `MaterialTheme` y componentes de Material 3 para mantener consistencia.
- **Adaptabilidad:** Diseñar interfaces que funcionen bien en diferentes tamaños de pantalla y factores de forma.
- **Safe Areas:** Utilizar `safeContentPadding()` o modificadores similares para respetar los notches y áreas del sistema en iOS y Android.
- **Componentes:** Toda la UI debe intentar utilizar componentes del paquete `components` y tender a crear componentes comunes y reutilizables.

## 4. Estado y Reactividad

- Usar `StateFlow` y `SharedFlow` para la comunicación entre ViewModel y UI.
- En la UI, observar el estado usando `collectAsStateWithLifecycle()` para una gestión eficiente del ciclo de vida.
- Evitar pasar ViewModels a componentes hijos; pasar estados y lambdas para eventos (State hoisting).

# 5. Recursos

- Utilizar el sistema de recursos de Compose Multiplatform (`composeResources`).
- Imágenes, strings y fuentes deben definirse en `commonMain/composeResources`.
- Acceder a ellos mediante la clase generada `Res`.

## 6- Navegación

- Utilizar `navigation-compose` en `commonMain`.
- Definir las rutas y el `NavHost` en un punto central de la aplicación compartida.

## 7. Convenciones de Código

- **Idiomas:** Código en inglés y comentarios en español.
- **Naming:**
    - `expect/actual`: Mantener el mismo nombre y paquete.
    - Composables: PascalCase.
    - Funciones/Variables: camelCase.
- **Dependency Injection:** Si se requiere, evaluar Koin o manual DI para mantener la simplicidad.
- **Imports de Previews:** Para asegurar la compatibilidad con el sistrema de previsualización de Compose Multiplatform, utilizar siempre:
    - `omport andoiidx.compose.ui.tooling.preview.Preview`
    - Evitar el uso de `org.jetbrains.compose.ui.tooling.preview.Preview`

## 8. Gestión de Dependencias

- Todas las dependencias deben gestionarse en `gradle/libs.versions.toml`.
- Evitar versiones "hardcoded" enn los archivos `buid.gladle.kts`.

## 9. Workflow de Desarrollo

- **Andoird:** Ejecutar normalmente desde Android Studio.
- **iOS:** Se puede ejecutar desde Android Studio (con el plugin de KMP) o abriendo el proyecto en `iosApp/` con Xcode para depuración especifica de la plataforma.

## 10. Organización de Paquetes

Se debe seguir una estrucutructura de paquetes clara y modular dentro de `commonMain`:

- `com.gdegilberto_android` (para mantener compatibilidad con el paquete de la aplicación original)
    - `ui`: Componentes visuales, pantallas (`screens`) y navegación.
    - `viewmodel`: Lógica de presentación y gestión de estado.
    - `domain`: Modelos de dominio y casos de uso.
    - `data`: Repositorios y fuentes de datos (API, base de datos local).
    - `di`: Configuración de injección de dependencias.
    - `util`: Funciones de utilidad y extensiones.
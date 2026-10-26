# Registro de vulnerabilidades

Este directorio distingue entre observaciones, hipótesis y vulnerabilidades confirmadas.

## Hallazgos iniciales

### F-01: Cookie de sesión sin HttpOnly

- **Evidencia disponible:** salida NSE de Nmap indica que `PHPSESSID` no tiene `HttpOnly`.
- **Estado:** observado, impacto pendiente de contextualizar.
- **Riesgo:** si un atacante consigue ejecutar JavaScript en el origen de la aplicación (por ejemplo, mediante XSS), la cookie podría quedar accesible desde ese JavaScript.
- **No demostrado todavía:** existencia de XSS, posibilidad de secuestro de sesión o ausencia de los atributos `Secure` y `SameSite`.
- **Remediación:** configurar cookies de sesión con `HttpOnly`, `Secure` cuando se use HTTPS y una política `SameSite` apropiada; corregir además cualquier XSS.

### F-02: Funcionalidad de subida de archivos

- **Evidencia disponible:** notas del usuario identifican `/panel` como formulario de subida y `/upload` o `/uploads` como posible ubicación de archivos.
- **Estado:** superficie de ataque identificada, explotación no documentada.
- **Riesgo potencial:** si se aceptan archivos ejecutables y el servidor los interpreta, un archivo subido podría ejecutarse con los permisos del proceso web. Esto es una hipótesis hasta documentar las validaciones y el comportamiento real.
- **Remediación:** lista permitida de extensiones, comprobación del contenido real, nombres generados por el servidor, límites de tamaño, almacenamiento fuera del directorio público, permisos mínimos y desactivación de ejecución en el directorio de cargas.

### F-03: CVE-2026-109618 / CVE-2026-109619

Consultar [análisis específico](CVE-2026-109618-109619.md). No atribuir estas CVE a RootMe hasta reunir evidencia suficiente de que el runtime PHP y el modo de ejecución están afectados.

## Estados usados

- **Observado:** aparece en una salida o captura.
- **Hipótesis:** posible explicación que aún requiere pruebas.
- **Confirmado:** evidencia reproducible y suficiente dentro del laboratorio.
- **No aplicable:** evidencia de que la condición necesaria no se cumple.
- **Pendiente:** faltan datos.

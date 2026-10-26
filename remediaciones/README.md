# Remediaciones recomendadas

Estas recomendaciones son preventivas y deberán ajustarse a los hallazgos que se confirmen en el laboratorio.

## 1. Cookies de sesión

- Establecer `HttpOnly` para evitar el acceso normal a la cookie desde JavaScript.
- Usar `Secure` en conexiones HTTPS.
- Configurar `SameSite` según las necesidades de la aplicación.
- Regenerar el identificador de sesión después de autenticar o elevar privilegios.
- Corregir posibles vulnerabilidades XSS mediante codificación contextual de salida, validación adecuada y una política CSP como defensa adicional.

La ausencia observada de `HttpOnly` no demuestra por sí sola que una sesión pueda secuestrarse.

## 2. Subida de archivos

- Usar una lista permitida de tipos y extensiones.
- Validar el contenido del archivo, no solo el nombre o el encabezado enviado por el cliente.
- Generar nombres de archivo del lado del servidor.
- Limitar tamaño, cantidad y frecuencia de cargas.
- Guardar las cargas fuera del directorio web público cuando sea posible.
- Si deben estar bajo el directorio web, impedir la ejecución de scripts en esa ubicación.
- Ejecutar el servicio con una cuenta sin privilegios administrativos.
- Aplicar permisos mínimos y registrar rechazos y errores de validación.

## 3. Servicios expuestos

- Mantener OpenSSH, Apache, PHP y el sistema operativo actualizados con los parches de seguridad de su proveedor.
- Desactivar módulos y servicios innecesarios.
- Restringir SSH mediante controles de acceso, autenticación fuerte y reglas de firewall apropiadas para el entorno.
- Revisar la configuración TLS y redirigir a HTTPS si el sitio procesa sesiones o datos sensibles.

Las versiones detectadas por banner no bastan para determinar si un paquete está sin parchear: las distribuciones pueden retroportar correcciones.

## 4. CVE-2026-109618 y CVE-2026-109619

- Inventariar todas las instalaciones y modos de ejecución de PHP.
- Aplicar versiones corregidas o los paquetes actualizados del proveedor.
- Reiniciar los workers tras actualizar.
- Si no se puede parchear inmediatamente, consultar las mitigaciones temporales oficiales y reducir la exposición; no considerarlas una solución definitiva.
- Vigilar logs, errores, reinicios inesperados y procesos creados por cuentas de servicio.

## 5. Evidencia de remediación

En una evaluación real, añadir evidencia de la configuración corregida y una nueva prueba que confirme el cambio. No afirmar que una remediación fue aplicada si solo se recomienda en este documento.

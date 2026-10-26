# Vulnerabilidades encontradas

## 1. Validación insegura de archivos subidos

- **Detección:** se encontró un formulario de subida en `/panel`.
- **Prueba realizada:** se cambió la extensión del archivo PHP a `.php5` y se obtuvo una shell inversa.
- **Impacto:** ejecución de código en el servidor y acceso inicial con los permisos de la cuenta web.
- **Remediación:** permitir solo tipos necesarios, validar el contenido real, almacenar los archivos fuera del directorio público y desactivar la ejecución de scripts en la carpeta de subidas.

## 2. Intérprete Python con SUID

- **Detección:** búsqueda de archivos propiedad de root con el permiso SUID.
- **Evidencia:** `/usr/bin/python2.7` pertenecía a `root` y tenía el permiso `-rwsr-xr-x`.
- **Validación:** Python informó UID efectivo `0` durante la prueba.
- **Impacto:** un usuario de bajo privilegio podía utilizar el intérprete para intentar obtener una shell con privilegios de root.
- **Remediación:** quitar el bit SUID a Python si no es necesario y revisar los permisos de todos los ejecutables SUID.

## 3. Cookie de sesión sin HttpOnly

- **Detección:** la salida de Nmap indicó que la cookie `PHPSESSID` no tenía `HttpOnly`.
- **Impacto posible:** la cookie podría ser accesible desde JavaScript si existiera una vulnerabilidad XSS.
- **Límite:** no se ha demostrado XSS ni secuestro de sesión.
- **Remediación:** usar `HttpOnly`, `Secure` con HTTPS y una política `SameSite` adecuada.

Los hallazgos anteriores corresponden a las observaciones y pruebas realizadas en el laboratorio.
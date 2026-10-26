# Escalada de privilegios

Durante la sesión obtenida a través del servidor web, se buscaron archivos con el bit SUID.

Se encontró:

```text
-rwsr-xr-x 1 root root ... /usr/bin/python2.7
```

El permiso SUID hace que el programa pueda iniciarse con el UID efectivo de su propietario. Como el propietario era `root`, se investigó Python como vía de escalada. La comprobación ejecutada con Python devolvió UID efectivo `0`.

La prueba permitió continuar el procedimiento del laboratorio hasta el acceso privilegiado. No se incluye la flag de `/root/root.txt`.

## Impacto

Si un intérprete accesible por usuarios sin privilegios tiene SUID de root y permite iniciar procesos privilegiados, puede provocar el compromiso completo del sistema.

## Remediación

- Eliminar el bit SUID de Python si no existe una necesidad administrativa justificada.
- Revisar periódicamente los archivos SUID y documentar cualquier excepción.
- Aplicar el principio de mínimo privilegio y limitar las cuentas de servicio.

## Evidencia pendiente

Añadir una captura propia de la salida de `id` con privilegios elevados, sin mostrar la flag completa ni información sensible.
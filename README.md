# TryHackMe: RootMe

## Objetivo

Resolver el laboratorio **RootMe** de TryHackMe, identificar las debilidades que permitieron avanzar y proponer medidas para prevenirlas.

- **Plataforma:** [TryHackMe: RootMe](https://tryhackme.com/room/rrootme)
- **Alcance:** máquina asignada dentro del entorno autorizado de TryHackMe.
- **Estado:** desafío completado según las pruebas realizadas. No se incluyen las flags completas.

## Herramientas utilizadas

- **Nmap:** reconocimiento de puertos y servicios.
- **Gobuster:** búsqueda de directorios web.
- **Navegador:** revisión de la página y del formulario de subida.
- **Netcat:** recepción de la conexión de shell inversa.

## Metodología y resultados

### 1. Reconocimiento

Se ejecutó:

```bash
nmap -sV -sC --min-rate 5000 10.66.140.66
```

Se encontraron dos puertos abiertos:

| Puerto | Servicio detectado |
|---|---|
| 22/tcp | SSH, OpenSSH 8.2p1 Ubuntu |
| 80/tcp | HTTP, Apache 2.4.41 (Ubuntu) |

La página web mostraba el título `HackIT - Home`. En la salida también se observó que la cookie `PHPSESSID` no tenía el atributo `HttpOnly`.

### 2. Enumeración web y acceso inicial

Con Gobuster y las notas del laboratorio se identificaron las rutas `/css`, `/js`, `/upload` y `/panel`. La ruta `/panel` contenía el formulario de subida y `/upload` correspondía al directorio relacionado con los archivos cargados.

En el panel se encontró un formulario de subida. Durante el laboratorio se modificó la extensión del archivo PHP a `.php5` y se consiguió una conexión de shell inversa. Esto indica que la validación de archivos y la configuración de ejecución del servidor permitían un archivo subido que podía ejecutar código.

### 3. Escalada de privilegios

En la sesión obtenida se buscó archivos con el permiso SUID. Se encontró `/usr/bin/python2.7`, propiedad de `root` y con el bit SUID activo. La comprobación de Python devolvió UID efectivo `0`, y se continuó con el procedimiento de la guía para obtener acceso privilegiado.

Más detalles: [escalada/README.md](escalada/README.md). El reconocimiento detallado y los resultados de Nmap y Gobuster están en [reconocimiento/README.md](reconocimiento/README.md).

## Vulnerabilidades e impacto

- **Validación insegura de archivos subidos:** permitió obtener ejecución de código a través del servidor web. En un entorno real, podría permitir acceso no autorizado al sistema con los permisos de la cuenta del servidor.
- **Python con SUID de root:** el intérprete podía ejecutarse con privilegios efectivos de root. Si una cuenta de bajo privilegio puede utilizarlo para iniciar una shell privilegiada, puede comprometerse todo el sistema.
- **Cookie sin `HttpOnly`:** aumenta el riesgo de exposición de la cookie si también existe una vulnerabilidad que permita ejecutar JavaScript en la página. Por sí sola, esta observación no demuestra que exista XSS.

## Remediaciones recomendadas

- Permitir únicamente tipos de archivo necesarios y comprobar tanto la extensión como el contenido.
- Guardar archivos subidos fuera del directorio público y deshabilitar la ejecución de scripts en esa ubicación.
- Quitar el bit SUID de intérpretes y programas que no lo necesiten; revisar periódicamente los archivos SUID.
- Configurar cookies de sesión con `HttpOnly`, `Secure` cuando se utilice HTTPS y `SameSite` apropiado.
- Mantener el sistema operativo y los servicios actualizados y aplicar el principio de mínimo privilegio.

## Evidencias

La carpeta [evidencias/](evidencias/README.md) indica las capturas y salidas que deben adjuntarse. No se publican flags completas, credenciales ni datos de sesión. Las capturas de ejecución deben añadirse desde el propio laboratorio, sin usar imágenes de guías externas como si fueran propias.

## Conclusión

El reconocimiento permitió identificar los servicios SSH y HTTP. La revisión de la web descubrió un formulario de subida que, debido a una validación insuficiente, permitió obtener una shell. Después, el hallazgo de Python con SUID permitió avanzar hacia privilegios de root. Las principales medidas preventivas son restringir las subidas, impedir la ejecución de archivos cargados y eliminar permisos SUID innecesarios.

## Referencias

- [TryHackMe: RootMe](https://tryhackme.com/room/rrootme)
- [Nmap](https://nmap.org/)
- [Gobuster](https://github.com/OJ/gobuster)

---
Informe educativo basado en las pruebas realizadas en el laboratorio autorizado.
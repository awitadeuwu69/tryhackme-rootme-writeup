# Reconocimiento inicial

## Escaneo Nmap

Comando informado:

```bash
nmap -sV -sC --min-rate 5000 10.66.140.66
```

### Resumen observado

| Puerto | Servicio | Banner |
|---|---|---|
| 22/tcp | SSH | OpenSSH 8.2p1 Ubuntu 4ubuntu0.13 |
| 80/tcp | HTTP | Apache httpd 2.4.41 (Ubuntu) |

También se reportó el título `HackIT - Home` y una cookie `PHPSESSID` sin el atributo `HttpOnly`.

### Interpretación y límites

- `-sV` intenta identificar versiones de servicios; los banners pueden ser incompletos o modificados.
- `-sC` ejecuta scripts NSE predeterminados.
- `--min-rate 5000` solicita una tasa mínima alta de envío de sondas. En laboratorios con latencia o limitación de tasa puede producir resultados menos fiables; conviene repetir el escaneo con una tasa más moderada si hay dudas.
- La salida compartida muestra solo los 1000 puertos TCP más habituales por defecto, no todos los puertos TCP posibles.
- La falta de `HttpOnly` puede aumentar el riesgo de lectura de la cookie mediante JavaScript si existe una vulnerabilidad de XSS, pero no demuestra por sí sola que exista XSS ni que la sesión sea explotable.
- La versión del banner de Apache no demuestra automáticamente que el servidor tenga una vulnerabilidad explotable: hay que comprobar parches del proveedor y configuración.

## Enumeración de directorios

Comando informado:

```bash
gobuster dir -u http://10.66.140.66 -w /usr/share/wordlists/dirb/common.txt
```

Rutas descritas en las notas:

| Ruta | Observación inicial | Pendiente |
|---|---|---|
| `/css` | Recursos de estilo | Confirmar código HTTP y contenido |
| `/js` | Recursos JavaScript | Confirmar código HTTP y contenido |
| `/uploads` | Posible directorio de archivos subidos | Confirmar que esta sea la ruta exacta; las notas originales decían `/upload` |
| `/panel` | Interfaz descrita como formulario de subida | Captura, código HTTP y controles observados |

No se han recibido aún la salida completa de Gobuster ni los códigos de estado. Por ello, las rutas se mantienen como datos por confirmar, no como vulnerabilidades demostradas.

## Próximos datos que añadir

- Salida íntegra de Nmap y Gobuster.
- Códigos HTTP de cada ruta.
- Si se identificó una versión de PHP, fuente fiable de esa identificación.
- Capturas sanitizadas de las páginas pertinentes.

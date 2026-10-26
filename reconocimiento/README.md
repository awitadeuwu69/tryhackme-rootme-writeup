# Reconocimiento inicial

## Escaneo Nmap

Comando ejecutado en el laboratorio:

```bash
nmap -sV -sC --min-rate 5000 10.66.140.66
```

### Resultados relevantes

El host respondió y Nmap informó 998 puertos TCP cerrados entre los 1000 puertos TCP habituales examinados.

| Puerto | Estado | Servicio | Versión detectada |
|---|---|---|---|
| 22/tcp | Abierto | SSH | OpenSSH 8.2p1 Ubuntu 4ubuntu0.13 |
| 80/tcp | Abierto | HTTP | Apache httpd 2.4.41 (Ubuntu) |

Otros datos observados:

- Título de la página: `HackIT - Home`.
- Cookie de sesión `PHPSESSID` sin el atributo `HttpOnly`.
- Sistema identificado por Nmap: Linux.

La ausencia de `HttpOnly` es una observación de configuración. Por sí sola no demuestra una vulnerabilidad XSS ni que una sesión pueda secuestrarse. Los banners de versión tampoco bastan para determinar por sí solos el estado de parcheo.

## Enumeración de directorios con Gobuster

Comando utilizado:

```bash
gobuster dir -u http://10.66.140.66 -w /usr/share/wordlists/dirb/common.txt
```

Rutas identificadas en las notas del laboratorio:

| Ruta | Observación |
|---|---|
| `/css` | Recursos de estilos de la página |
| `/js` | Recursos JavaScript |
| `/upload` | Directorio relacionado con los archivos subidos |
| `/panel` | Panel con formulario para subir archivos |

La ruta `/panel` permitió localizar el formulario de subida. En las pruebas del laboratorio, la extensión `.php` fue rechazada y el archivo pudo subirse tras cambiar su extensión a `.php5`. La ejecución de la shell inversa debe respaldarse con la captura propia incluida en la sección de evidencias.

## Límites de los resultados

- El escaneo mostrado cubre los 1000 puertos TCP habituales por defecto, no todos los puertos TCP posibles.
- `--min-rate 5000` solicita una tasa de envío alta; si faltaran resultados o hubiera dudas, convendría repetir el escaneo con una tasa más moderada.
- La salida original completa de Gobuster y sus códigos HTTP no están incluidos aquí, por lo que las rutas se describen según las notas tomadas durante el laboratorio.

## Referencias

- [TryHackMe: RootMe](https://tryhackme.com/room/rrootme)
- [Nmap](https://nmap.org/)
- [Gobuster](https://github.com/OJ/gobuster)
- [PentestMonkey PHP reverse shell](https://github.com/pentestmonkey/php-reverse-shell)
- [Guía de referencia consultada sobre RootMe](https://medium.com/@shubamagrawal112/tryhackme-rootme-1fca0c4e0669)

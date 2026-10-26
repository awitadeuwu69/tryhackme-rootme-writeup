# TryHackMe: RootMe

> **Estado:** informe en progreso. Este documento se basa únicamente en las observaciones compartidas hasta ahora. Los pasos de explotación, las flags y la escalada de privilegios todavía no están documentados ni verificados.

## 1. Objetivo y alcance

Documentar la evaluación práctica de la máquina **RootMe** de TryHackMe, registrando reconocimiento, posibles vulnerabilidades, validaciones, evidencias y recomendaciones de remediación.

- Plataforma: [TryHackMe · RootMe](https://tryhackme.com/room/rrootme)
- Objetivo observado durante el escaneo: `10.66.140.66` (IP de laboratorio, puede cambiar).
- Alcance: únicamente la máquina asignada dentro del laboratorio autorizado de TryHackMe.
- Estado de finalización: **pendiente de confirmar**. No se afirma que el CTF esté completado.

## 2. Entorno y herramientas

- Nmap para enumeración de puertos y versiones.
- Gobuster para descubrir rutas web.
- Navegador web para inspeccionar la aplicación.
- Sistema operativo del atacante: pendiente de documentar.
- Fecha y hora del escaneo según la salida proporcionada: 25 de octubre de 2026, 21:15 (zona indicada por Nmap: UTC−04:00).

## 3. Reconocimiento

### 3.1 Escaneo de servicios

Comando ejecutado:

```bash
nmap -sV -sC --min-rate 5000 10.66.140.66
```

Resumen de la salida aportada:

| Puerto | Estado | Servicio | Versión detectada |
|---|---|---|---|
| 22/tcp | Abierto | SSH | OpenSSH 8.2p1 Ubuntu 4ubuntu0.13 |
| 80/tcp | Abierto | HTTP | Apache httpd 2.4.41 (Ubuntu) |

Otros datos observados:
- Título HTTP: `HackIT - Home`.
- Cookie `PHPSESSID` sin el atributo `HttpOnly` en la respuesta revisada.
- Sistema operativo inferido por Nmap: Linux.
- La identificación de versión por banner no demuestra por sí sola que un servicio sea explotable.

Véase [reconocimiento/README.md](reconocimiento/README.md).

### 3.2 Enumeración web

Se informó de la ejecución de Gobuster:

```bash
gobuster dir -u http://10.66.140.66 -w /usr/share/wordlists/dirb/common.txt
```

Rutas reportadas:
- `/css`: recursos de estilo.
- `/js`: recursos JavaScript.
- `/uploads`: directorio reportado como posible destino de archivos subidos; confirmar la ruta exacta y respuesta HTTP.
- `/panel`: interfaz reportada para subir archivos.

Las rutas se consideran observaciones iniciales, no vulnerabilidades confirmadas. Faltan el resultado completo de Gobuster, códigos HTTP y evidencias visuales.

## 4. Vulnerabilidades y hallazgos

| ID | Hallazgo | Estado actual | Prioridad provisional |
|---|---|---|---|
| F-01 | Cookie de sesión sin `HttpOnly` | Observado en la salida de Nmap; requiere comprobar si la cookie es sensible y revisar las demás directivas | Baja / por validar |
| F-02 | Funcionalidad de subida de archivos en `/panel` y posible directorio `/uploads` | Superficie de ataque identificada; aún no se ha documentado una validación de archivo débil | Por determinar |
| F-03 | CVE-2026-109618 y CVE-2026-109619 (PHP) | Investigación complementaria; no se ha confirmado versión, SAPI ni vulnerabilidad en esta máquina | Crítica si se confirma exposición, no atribuida todavía al objetivo |

El análisis separado de las CVE se encuentra en [vulnerabilidades/CVE-2026-109618-109619.md](vulnerabilidades/CVE-2026-109618-109619.md).

## 5. Explotación y escalada de privilegios

**Pendiente de completar con la experiencia real.** Añadir aquí, en orden:
1. Condición vulnerable y cómo se verificó.
2. Herramientas y comandos ejecutados.
3. Evidencia que demuestra el resultado.
4. Usuario o contexto de ejecución obtenido.
5. Procedimiento de escalada, si se realizó, y evidencia de los privilegios alcanzados.

No se debe presentar un procedimiento encontrado en una guía externa como si se hubiera ejecutado personalmente. No publicar flags completas, contraseñas, tokens, claves privadas ni datos de sesión.

## 6. Evidencias

Consultar [evidencias/README.md](evidencias/README.md) para la lista de capturas y resultados pendientes. Antes de publicar capturas, ocultar flags, credenciales, cookies de sesión y cualquier dato sensible.

## 7. Remediaciones

Las recomendaciones iniciales están en [remediaciones/README.md](remediaciones/README.md). Las medidas definitivas deben corresponder a los hallazgos que se logren confirmar.

## 8. Conclusiones

El reconocimiento inicial detectó SSH en el puerto 22 y Apache HTTP en el puerto 80, además de una aplicación web que utiliza una cookie denominada `PHPSESSID` y una zona de subida de archivos reportada por enumeración. Estos datos ayudan a priorizar la investigación, pero no bastan para afirmar una explotación exitosa.

El informe se actualizará cuando se incorporen los resultados reales de validación, acceso inicial, escalada y finalización del laboratorio.

## 9. Referencias

- [TryHackMe: RootMe](https://tryhackme.com/room/rrootme)
- [Nmap Reference Guide](https://nmap.org/book/man.html)
- [Gobuster](https://github.com/OJ/gobuster)
- [PHP: archivo de avisos de seguridad de 2026](https://www.php.net/archive/2026.php)
- [NCSC del Reino Unido: vulnerabilidades críticas de PHP](https://www.ncsc.gov.uk/news/critical-security-issue-affecting-php)

---
**Nota ética:** documentación educativa de un entorno autorizado. No ejecutar pruebas contra sistemas ajenos al alcance definido.

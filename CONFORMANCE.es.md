# AUM-IC Sanitizer - Especificacion de Conformidad y Verificacion

**Estado:** Norma Tecnica / Especificacion de Verificacion  
**Autoridad Emisora:** Ingenieria Creativa (IngCrea)  
**Identificador Normativo:** AUM-IC-SANITIZER-VERIF-1:2026  
**Norma Madre:** AUM-IC 7:2026 (AUM-IC-SPEC-7:2026)  
**Licencia:** Licencia MIT

---

## 1. Alcance
El presente documento especifica el funcionamiento mecanico, los pases de inspeccion y las reglas de mutacion automatizada que ejecuta el **AUM-IC Sanitizer (`@ingcrea/aumic-sanitizer`)** para garantizar la conformidad objetiva del repositorio con el estandar AUM-IC 7:2026.

## 2. Pases de Conformidad Automatizados

### 2.1. Pase 1: Aplanamiento POSIX de Saltos de Linea (Clausula 3.1)
- El validador **DEBE** escanear todos los flujos de texto UTF-8 y sustituir cualquier byte Carriage Return (CR, `0x0D`) que anteceda a un Line Feed (LF, `0x0A`) por un byte LF unitario (`0x0A`).
- El validador **DEBE** rechazar o rectificar automaticamente cualquier caracter CR (`0x0D`) aislado.

### 2.2. Pase 2: Auditoria Forense Hexadecimal y Mojibake (Clausula 3.2)
- El motor **DEBE** ejecutar evaluaciones de expresiones regulares a nivel de byte contra secuencias de corrupcion Windows-1252 (ej. `ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â¡`, `ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â©`, `ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â³`, `ÃƒÆ’Ã†â€™Ãƒâ€šÃ‚Â±`, `ÃƒÆ’Ã‚Â¢ÃƒÂ¢Ã¢â‚¬Å¡Ã‚Â¬ÃƒÂ¢Ã¢â€šÂ¬Ã‚Â`).
- El motor **DEBE** purgar puntos de control de ancho cero (U+200B, U+200C, U+200D y U+FEFF en flujos no iniciales).

### 2.3. Pase 3: Higiene de Codigo en Produccion (Clausula 3.4)
- Cuando se invoque en modo estricto o en contexto pre-commit, el validador **DEBE** eliminar o reportar llamadas activas a `console.log()`, sentencias `debugger;` y marcadores huerfanos de depuracion fuera de directorios de prueba.

## 3. Codigos de Salida y Contratos de Barrera
- **Codigo de Salida 0:** Conformidad verificada (repositorio limpio o auto-sanado con exito).
- **Codigo de Salida 1:** No-conformidad fatal detectada bajo modo de inspeccion (`--check`), bloqueando el commit de Git o la etapa del pipeline CI/CD.\n
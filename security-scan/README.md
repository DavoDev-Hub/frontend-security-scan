# Análisis de seguridad con Nuclei — frontend

## Cómo correrlo

1. Levanta el stack (si no está corriendo):
   ```sh
   docker compose up -d --build
   ```
2. Corre el escaneo:
   ```sh
   ./security-scan/run-nuclei.sh
   ```
   (por defecto apunta al servicio `frontend`, puerto 80 — el nginx)

El contenedor de Nuclei se conecta con `--network container:<id del contenedor>`,
es decir, comparte el namespace de red del contenedor `frontend` directamente.
Así no depende de que el puerto esté publicado al host ni de adivinar el
nombre de la red de `docker-compose` — es el "bridge" entre Nuclei y lo que
se está analizando.

3. El resultado queda en `security-scan/results/nuclei-frontend.txt`.

## Flujo de entrega

1. Correr el escaneo y guardar el resultado en esta carpeta.
2. `git add security-scan/ && git commit -m "Agrega resultados de análisis Nuclei" && git push`.
3. Revisar cada hallazgo:
   - Si es de complejidad alta y el profesor indica que no se debe
     implementar, se deja documentado aquí (con la razón) y no se toca.
   - El resto se corrige **uno por uno**, con un commit independiente por
     cada corrección, para que el historial del repo muestre claramente
     qué se arregló y por qué (ej. `git commit -m "Corrige cabecera
     X-Frame-Options faltante (hallazgo Nuclei: missing-security-headers)"`).

## Notas

- Nuclei es un escáner de vulnerabilidades HTTP (headers de seguridad
  faltantes, exposición de archivos/paneles, configuraciones por defecto,
  CVEs conocidos en el software detectado, etc.). No todo lo que reporta es
  explotable en este laboratorio — hay que revisar cada hallazgo antes de
  "corregirlo a ciegas".
- Si el escaneo no encuentra nada, igual se documenta aquí (archivo de
  resultado vacío o con "no issues found") como evidencia de que se corrió.

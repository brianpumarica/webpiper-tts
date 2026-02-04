## Docker command

```bash
docker-compose up -d
```

## URL dinamic to test

```bash
cloudflared tunnel --url http://localhost:8080
```

## Nodos HTTP en n8n

### 1. Generar Audio (POST)

En lugar de poner la clave, usamos `usuario:contraseña` como indicativo genérico.

```bash
curl -X POST "https://TU-URL-CLOUDFLARE/api/v1/synthesize" \
  -u usuario:contraseña \
  -H "Content-Type: application/json" \
  -d '{
    "text": "Texto de prueba",
    "voice": "es_ES-davefx-medium",
    "local": "es_ES"
  }'

```

### 2. Descargar Audio (GET)

```bash
curl -X GET "https://TU-URL-CLOUDFLARE/api/v1/synthesize/NOMBRE_DEL_ARCHIVO.wav" \
  -u usuario:contraseña \
  -o audio_descargado.wav

```

**Nota sobre Autenticación:**
Este servicio requiere **Basic Auth**. Asegúrate de reemplazar `usuario:contraseña` en los comandos anteriores con las credenciales configuradas en tu variable de entorno `API_KEY` (por defecto el usuario es `piper`).
## 🚀 Despliegue Rápido con Docker

```bash
docker-compose up -d
```

## 🌐 Túnel para Pruebas Externas (Cloudflare)

Si necesitas probar los webhooks desde fuera de tu red local:

```bash
# Nota: Usamos el puerto 8081 que es el expuesto en el docker-compose
cloudflared tunnel --url http://localhost:8081

```

## 🛠 Configuración de Nodos HTTP (n8n / API)

El servicio utiliza **Basic Auth** para seguridad.

* **Usuario:** `piper` (por defecto)
* **Contraseña:** El valor que hayas puesto en `API_KEY` en tu `docker-compose.yml`.

---

## ⚡ Automatización con n8n

Para mantener el flujo ordenado y fácil de editar, recomendamos usar un nodo **"Edit Fields" (Set)** al inicio del flujo para definir todas las variables.

### 1. Nodo de Configuración (Set)

Crea un nodo **Edit Fields** al inicio y define estas variables. Así no tendrás que tocar los nodos HTTP nunca más.

| Nombre de Variable | Tipo | Valor (Ejemplo) | Descripción |
| --- | --- | --- | --- |
| `base_url` | String | `https://tu-url.trycloudflare.com` | Tu URL del túnel (sin barra al final) |
| `voice_model` | String | `es_ES-davefx-medium` | Modelo de voz a utilizar |
| `text_input` | String | `Hola mundo` | El texto a convertir |

### 2. Nodo HTTP: Generar Audio (POST)

Configura el nodo HTTP referenciando las variables del paso anterior.

* **Method:** `POST`
* **URL:** `{{ $json.base_url }}/api/v1/synthesize`
* **Authentication:** Basic Auth


* **Body Content:** JSON
```json
{
  "text": "{{ $json.text_input }}",
  "voice": "{{ $json.voice_model }}",
  "local": "es_ES"
}

```



### 3. Nodo HTTP: Descargar Audio (GET)

Este nodo tomará la respuesta del anterior (que suele ser la ruta del archivo) y la descargará.

* **Method:** `GET`
* **URL:** `{{ $json.base_url }}/api/v1/synthesize/{{ $json.nombre_archivo_recibido }}`

---

## 📁 Estructura de Archivos

Al levantar el contenedor, se gestionarán estas carpetas en tu directorio local:

* `/output`: Aquí se guardarán los archivos `.wav` generados.
* `/voices`: Aquí puedes añadir modelos de voz adicionales (`.onnx` y `.json`).

---

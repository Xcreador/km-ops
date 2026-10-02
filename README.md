# km-ops

Infraestructura operativa publica de KM Beauty. **No contiene secretos, credenciales, datos de clientes ni codigo propietario.**

## KM Cron (produccion)

`.github/workflows/km-cron-produccion.yml` sustituye al WP-Cron publico de la tienda (One.com no ofrece cron del servidor):

- cada 5 minutos (y a mano con *Run workflow*) hace un `POST` firmado a `/wp-json/km-cron/v1/run`;
- firma: cabecera `X-KM-Cron: KMCron t=<unix>,n=<nonce>,s=<HMAC-SHA256(secreto, "t.n")>` (marca de tiempo + nonce de un solo uso);
- el secreto vive solo en *Settings -> Secrets and variables -> Actions* como `KM_CRON_SECRET_PROD` y nunca se imprime;
- los registros solo muestran el codigo HTTP; 200 = ejecutado, 429 = intervalo minimo (no es error).

Rotacion del secreto: generar uno nuevo en el panel de WordPress (Herramientas -> Cron KM) y actualizar el secret del repositorio.

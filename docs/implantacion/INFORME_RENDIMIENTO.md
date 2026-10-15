# INFORME_RENDIMIENTO — Resiliencia, Pruebas de Carga y Chaos Engineering Local

## 1. Tabla Comparativa de Resultados

| Fase | Servidor / Arquitectura | Límite CPU | Usuarios Concurrentes | RPS Logrados | Latencia P95 (ms) | % Errores |
|------|--------------------------|------------|------------------------|--------------|-------------------|-----------|
| Fase 1 | Flask Dev (app.run) | 0.20 Cores | 50 | _pendiente_ | _pendiente_ | _pendiente_ |
| Fase 2 | Gunicorn (4 workers) | 0.20 Cores | 50 | _pendiente compañero_ | _pendiente compañero_ | _pendiente compañero_ |
| Fase 3 | Nginx + 3 Réplicas | 0.20 c/u | 150 | _pendiente compañero_ | _pendiente compañero_ | _pendiente compañero_ |

> Fase 1: `locust` con Host `http://localhost:5000`, 50 usuarios, spawn rate 5.
> Fase 2: misma prueba, Host `http://localhost:5000`.
> Fase 3: Host `http://localhost` (puerto 80 Nginx), 150 usuarios + `docker stop <réplica>`.

### Observaciones Fase 1 (responsable Fase 1)

- Segundos hasta saturar 20% CPU (`docker stats`): _pendiente medición con Docker_
- Momento del primer timeout/error: _pendiente medición con Docker_
- Evidencia local sin Docker (referencial, NO oficial): RPS ~35.8, P95 ~72 ms, 0% errores en 2 min (ver `fase1_stats.csv` si se adjunta).

## 2. Captura Fase 3 (compañeros — NO tocar)

- `docs/implantacion/fase3-chaos.png` — pendiente: gráfica de Locust en el momento exacto del `docker stop`.

## 3. Análisis Técnico (compañeros — NO tocar, máx. 2 párrafos)

Pendiente: ¿Por qué un servidor multiproceso con balanceador logra más tráfico con las mismas restricciones que un uniproceso?

## 4. Archivos base verificados (Fase 1)

- `Dockerfile` → `CMD ["python", "app.py"]`
- `docker-compose.yml` → servicio `api-backend`, `cpus: '0.20'`, `memory: 128M`
- `requirements.txt` → flask + gunicorn
- `locustfile.py` → endpoints existentes en `app.py`: `GET /api/health`, `GET /api/products`, `POST /api/products`, `POST /api/orders`
- `nginx.conf` → creado según guía (se usa en Fase 3)

### Переменные окружения

| Переменная | Назначение | Значение по умолчанию |
|---|---|---|
| APP_ENV | режим работы backend | production |
| VUE_APP_API_URL | URL backend для frontend | http://localhost:8081 |
| DOCKER_USER | Docker Hub username | local |
| IMAGE_TAG | тег образа | latest |

### Оптимизация образов

Backend и frontend собираются через multi-stage builds.  
В финальные образы не попадают инструменты сборки Go, Node.js и npm-зависимости для разработки.

Backend:
- builder: golang alpine;
- runtime: alpine.

Frontend:
- builder: node alpine;
- runtime: nginx unprivileged alpine.

### Проверка

```bash
docker compose build
docker compose up -d
docker compose ps
curl -s -o /dev/null -w "%{http_code}\n" http://localhost:8081/health
curl http://localhost
# COMUNICACION-CONTAINER-HOST

Escenario:

Tienes un servicio corriendo en tu HOST (ej: base de datos en localhost:5432) y necesitas que un container se conecte a él.

Tareas:
# 3.1 - Simula un servicio en el host:

```bash

# 1. Inicia el "servicio" en el host usando Docker
docker run -d --name host-server -p 8888:80 nginx

# 2. Verifica que está corriendo
curl localhost:8888
# Deberías ver la página de bienvenida de nginx

```

# 3.2 - Intenta conectarte SIN configuración especial:

```bash

# En otra terminal, ejecuta:
docker run --rm alpine sh -c "wget -O- http://localhost:8888"

# ¿Funciona? ¿Por qué no?

Connecting to localhost:8888 ([::1]:8888)
wget: can't connect to remote host: Connection refused

```

# 3.3 - Solución 1: host.docker.internal (Docker Desktop):

```bash

# Si estás en Docker Desktop (Mac/Windows):
docker run --rm alpine sh -c "wget -O- http://host.docker.internal:8888"

# ¿Funciona ahora?

Connecting to host.docker.internal:8888

```

# 3.4 - Solución 2: --add-host (Linux):

```bash

# En Linux:
docker run --rm --add-host=host.docker.internal:host-gateway alpine sh -c "wget -O- http://host.docker.internal:8888"

# ¿Funciona?

Connecting to host.docker.internal:8888

```

# 3.5 - Solución 3: Host network (sin aislamiento):

```bash

docker run --rm --network host alpine sh -c "wget -O- http://localhost:8888"

# ¿Funciona? ¿Por qué?
# ¿Cuál es el trade-off de usar --network host?

Connecting to localhost:8888 ([::1]:8888)
wget: can't connect to remote host: Connection refused

```
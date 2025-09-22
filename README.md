# Taller_Traefick_middlewheres

TALLER Gateway de servicios con Traefik 1.

    Los host cofigurados para el enrrutamineto de traefik son:

    ops.localhost: Usado para acceder al dashboard de Traefik.

    api.localhost: Usado para acceder a la API de la aplicación.

    Comprobaciones 3.1. API en api.localhost responde a /health image 3.2. Dashboard accesible solo en ops.localhost/dashboard/ y con autenticación 3.3. Servicios y Routers visibles en el Dashboard 3.4. Rate-limit o stripPrefix aplicado y verificado: Aca se demuestra la efectividad del reateLimit con un buvle de 300 peticiones donde no pudo responde 133 que esta en falied por el rate de 100 que le interpusimos
    daniel-reyes@daniel-reyes-VirtualBox:~/traefik-middleware$ ab -n 200 -c 100 http://api.localhost/ This is ApacheBench, Version 2.3 <$Revision: 1903618 $> Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/ Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking api.localhost (be patient) Completed 100 requests Completed 200 requests Finished 200 requests

Server Software: nginx/1.29.1 Server Hostname: api.localhost Server Port: 80

Document Path: / Document Length: 617 bytes

Concurrency Level: 100 Time taken for tests: 0.551 seconds Complete requests: 200 Failed requests: 133 (Connect: 0, Receive: 0, Length: 133, Exceptions: 0) Non-2xx responses: 200 Total transferred: 81356 bytes HTML transferred: 43600 bytes Requests per second: 363.02 [#/sec] (mean) Time per request: 275.466 [ms] (mean) Time per request: 2.755 [ms] (mean, across all concurrent requests) Transfer rate: 144.21 [Kbytes/sec] received

Connection Times (ms) min mean[+/-sd] median max Connect: 0 7 7.5 2 19 Processing: 2 98 78.4 110 292 Waiting: 1 96 78.7 109 291 Total: 2 105 83.1 121 307

Percentage of the requests served within a certain time (ms) 50% 121 66% 136 75% 155 80% 164 90% 232 95% 281 98% 300 99% 301 100% 307 (longest request) daniel-reyes@daniel-reyes-VirtualBox:~/traefik-middleware$ image

3.5.Balanceo entre 2 réplicas evidenciado: Aca se mostro como se ivan alternando las dos replicas del backend para responder las peticiones y es evidenciable en el hostname que es diferente entre si daniel-reyes@daniel-reyes-VirtualBox:~/traefik-middleware$ for i in {1..10}; do curl http://api.localhost/v1/personas; echo ""; done { "data": [ { "edad": 26, "id": "34189ec4-61dc-4f5c-aadb-5c556049a5b8", "nombre": "Daniel" }, { "edad": 69, "id": "2753c95a-a34c-4c28-82ce-2a0e6b808059", "nombre": "Camila" } ], "has_next": false, "has_prev": false, "hostname": "3c53294fcb50", "page": 1, "per_page": 50, "total_pages": 1, "total_records": 2 }

{ "data": [ { "edad": 26, "id": "34189ec4-61dc-4f5c-aadb-5c556049a5b8", "nombre": "Daniel" }, { "edad": 69, "id": "2753c95a-a34c-4c28-82ce-2a0e6b808059", "nombre": "Camila" } ], "has_next": false, "has_prev": false, "hostname": "b0d5d25da71f", "page": 1, "per_page": 50, "total_pages": 1, "total_records": 2 }

{ "data": [ { "edad": 26, "id": "34189ec4-61dc-4f5c-aadb-5c556049a5b8", "nombre": "Daniel" }, { "edad": 69, "id": "2753c95a-a34c-4c28-82ce-2a0e6b808059", "nombre": "Camila" } ], "has_next": false, "has_prev": false, "hostname": "3c53294fcb50", "page": 1, "per_page": 50, "total_pages": 1, "total_records": 2 }

{ "data": [ { "edad": 26, "id": "34189ec4-61dc-4f5c-aadb-5c556049a5b8", "nombre": "Daniel" }, { "edad": 69, "id": "2753c95a-a34c-4c28-82ce-2a0e6b808059", "nombre": "Camila" } ], "has_next": false, "has_prev": false, "hostname": "b0d5d25da71f", "page": 1, "per_page": 50, "total_pages": 1, "total_records": 2 }

{ "data": [ { "edad": 26, "id": "34189ec4-61dc-4f5c-aadb-5c556049a5b8", "nombre": "Daniel" }, { "edad": 69, "id": "2753c95a-a34c-4c28-82ce-2a0e6b808059", "nombre": "Camila" } ], "has_next": false, "has_prev": false, "hostname": "3c53294fcb50", "page": 1, "per_page": 50, "total_pages": 1, "total_records": 2 }

{ "data": [ { "edad": 26, "id": "34189ec4-61dc-4f5c-aadb-5c556049a5b8", "nombre": "Daniel" }, { "edad": 69, "id": "2753c95a-a34c-4c28-82ce-2a0e6b808059", "nombre": "Camila" } ], "has_next": false, "has_prev": false, "hostname": "b0d5d25da71f", "page": 1, "per_page": 50, "total_pages": 1, "total_records": 2 }

{ "data": [ { "edad": 26, "id": "34189ec4-61dc-4f5c-aadb-5c556049a5b8", "nombre": "Daniel" }, { "edad": 69, "id": "2753c95a-a34c-4c28-82ce-2a0e6b808059", "nombre": "Camila" } ], "has_next": false, "has_prev": false, "hostname": "3c53294fcb50", "page": 1, "per_page": 50, "total_pages": 1, "total_records": 2 }

{ "data": [ { "edad": 26, "id": "34189ec4-61dc-4f5c-aadb-5c556049a5b8", "nombre": "Daniel" }, { "edad": 69, "id": "2753c95a-a34c-4c28-82ce-2a0e6b808059", "nombre": "Camila" } ], "has_next": false, "has_prev": false, "hostname": "b0d5d25da71f", "page": 1, "per_page": 50, "total_pages": 1, "total_records": 2 }

{ "data": [ { "edad": 26, "id": "34189ec4-61dc-4f5c-aadb-5c556049a5b8", "nombre": "Daniel" }, { "edad": 69, "id": "2753c95a-a34c-4c28-82ce-2a0e6b808059", "nombre": "Camila" } ], "has_next": false, "has_prev": false, "hostname": "3c53294fcb50", "page": 1, "per_page": 50, "total_pages": 1, "total_records": 2 }

{ "data": [ { "edad": 26, "id": "34189ec4-61dc-4f5c-aadb-5c556049a5b8", "nombre": "Daniel" }, { "edad": 69, "id": "2753c95a-a34c-4c28-82ce-2a0e6b808059", "nombre": "Camila" } ], "has_next": false, "has_prev": false, "hostname": "b0d5d25da71f", "page": 1, "per_page": 50, "total_pages": 1, "total_records": 2 }

daniel-reyes@daniel-reyes-VirtualBox:~/traefik-middleware$
image

Bonus Parcial

Paginar de error 404 personalizada image

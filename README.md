# Introducción a Herramientas DevOps ISY1101

**Introducción a Herramientas DevOps** es una asignatura del quinto semestre de la carrera Ingeniería en Informática, mención Desarrollo de Software. En ella vas a comprender y aplicar los fundamentos de la metodología DevOps, con énfasis en la automatización del ciclo de vida del software y la colaboración entre los equipos de desarrollo y operaciones. A través de actividades prácticas en entornos cloud, principalmente sobre Amazon Web Services (AWS), trabajarás la configuración de infraestructura básica, la contenedorización de aplicaciones mediante Docker y la automatización de despliegues con herramientas de integración y entrega continua (CI/CD). La evaluación incluye instancias formativas y sumativas centradas en laboratorios y ejercicios prácticos, orientados a prepararte para contextos profesionales asociados al uso de herramientas DevOps.

- [Introducción a Herramientas DevOps ISY1101](#introducción-a-herramientas-devops-isy1101)
  - [Objetivo de la guía](#objetivo-de-la-guía)
  - [Desarrollo de las actividades](#desarrollo-de-las-actividades)
    - [Actividad 2.3.2 - Desplegar la app web en Docker y en cada EC2 por capas](#actividad-232---desplegar-la-app-web-en-docker-y-en-cada-ec2-por-capas)
      - [Enunciado](#enunciado)
      - [Actividades](#actividades)
        - [Paso 1 - Implementando la VPC](#paso-1---implementando-la-vpc)
          - [Paso 1.1 - Crear la VPC](#paso-11---crear-la-vpc)
          - [Paso 1.2 - Crear subredes](#paso-12---crear-subredes)
          - [Paso 1.3 - Crear la puerta de enlace de internet](#paso-13---crear-la-puerta-de-enlace-de-internet)
          - [Paso 1.4 - Crear la puerta de enlace NAT](#paso-14---crear-la-puerta-de-enlace-nat)
          - [Paso 1.5 - Crear las tablas de enrutamiento](#paso-15---crear-las-tablas-de-enrutamiento)
          - [Paso 1.6 - Crear los grupos de seguridad](#paso-16---crear-los-grupos-de-seguridad)
        - [Paso 2 - Implementando las instancias EC2](#paso-2---implementando-las-instancias-ec2)
          - [Paso 2.1 - Crear par de claves](#paso-21---crear-par-de-claves)
          - [Paso 2.2 - Crear la plantilla de lanzamiento](#paso-22---crear-la-plantilla-de-lanzamiento)
          - [Paso 2.3 - Lanzar instancias de EC2](#paso-23---lanzar-instancias-de-ec2)
          - [Paso 2.4 - Conectarse a las instancias](#paso-24---conectarse-a-las-instancias)
        - [Paso 3 -Desplegar la aplicación](#paso-3--desplegar-la-aplicación)
          - [Paso 3.1 - Copiar los archivos](#paso-31---copiar-los-archivos)
          - [Paso 3.2 - Desplegar la base de datos](#paso-32---desplegar-la-base-de-datos)
          - [Paso 3.3 - Desplegar el backend](#paso-33---desplegar-el-backend)
          - [Paso 3.4 - Desplegar el frontend](#paso-34---desplegar-el-frontend)
    - [Actividad 2.4.2 - Gestión y versionado de imágenes en Amazon ECR y Docker Hub](#actividad-242---gestión-y-versionado-de-imágenes-en-amazon-ecr-y-docker-hub)
      - [Enunciado](#enunciado-1)
      - [Actividades](#actividades-1)
        - [Actividad 1 - Publicación de imágenes en Docker Hub](#actividad-1---publicación-de-imágenes-en-docker-hub)
          - [Paso 1 - Crear cuenta en Docker Hub](#paso-1---crear-cuenta-en-docker-hub)
          - [Paso 2 - Desplegar la imagen de `tienda-db`](#paso-2---desplegar-la-imagen-de-tienda-db)
          - [Paso 3 - Desplegar la imagen de `tienda-backend`](#paso-3---desplegar-la-imagen-de-tienda-backend)
          - [Paso 3 - Desplegar la imagen de `tienda-frontend`](#paso-3---desplegar-la-imagen-de-tienda-frontend)
          - [Paso 5 - Verificar](#paso-5---verificar)
        - [Actividad 2 - Publicación de imágenes en Amazon ECR](#actividad-2---publicación-de-imágenes-en-amazon-ecr)
          - [Paso 1 - Crear los repositorios en ECR](#paso-1---crear-los-repositorios-en-ecr)
          - [Paso 2 - Autenticarse desde Docker a ECR](#paso-2---autenticarse-desde-docker-a-ecr)
    - [Actividad 2.5.2 - Construye y despliegue pipeline CICD con GitHub Actions](#actividad-252---construye-y-despliegue-pipeline-cicd-con-github-actions)
      - [Enunciado](#enunciado-2)
      - [Actividades](#actividades-2)
        - [Paso 1 - Preparar el repositorio en GitHub](#paso-1---preparar-el-repositorio-en-github)
        - [Paso 2 - Configurar secretos en GitHub (Settings → Secrets and variables → Actions)](#paso-2---configurar-secretos-en-github-settings--secrets-and-variables--actions)
        - [Paso 3 - Ejecutar la práctica en clase](#paso-3---ejecutar-la-práctica-en-clase)
        - [Paso 4 - Validar el despliegue en AWS](#paso-4---validar-el-despliegue-en-aws)
  - [Anexo](#anexo)
    - [Comandos útiles de Docker](#comandos-útiles-de-docker)
    - [Comandos útiles de Docker Compose](#comandos-útiles-de-docker-compose)
  - [Autor](#autor)
  - [Licencia](#licencia)

## Objetivo de la guía

Esta guía te presenta una solución a las actividades **2.3.2**, **2.4.2** y **2.5.2** de la **experiencia de aprendizaje 2**, correspondiente a la asignatura **Introducción a Herramientas DevOps ISY1101**.

## Desarrollo de las actividades

### Actividad 2.3.2 - Desplegar la app web en Docker y en cada EC2 por capas

#### Enunciado

En esta actividad vas a **desplegar una aplicación web multicapa utilizando contenedores Docker**, distribuyendo cada componente (**frontend**, **backend** y **base de datos**) en **instancias EC2 separadas**, de acuerdo con el diseño de arquitectura por capas definido previamente.

Para ello, te conectarás a las instancias EC2 mediante **Session Manager**, instalarás y validarás el servicio Docker, construirás las **imágenes Docker** de cada componente y ejecutarás los **contenedores** con la configuración necesaria para asegurar la comunicación entre capas, utilizando direcciones IP privadas y variables de entorno.

Como parte del proceso, deberás **verificar el acceso a la aplicación desde el navegador**, validar la correcta conexión entre frontend, backend y base de datos, y **revisar los logs de los contenedores** para comprobar el funcionamiento de cada servicio. Esta actividad te permitirá entender cómo se implementa una arquitectura distribuida basada en contenedores sobre infraestructura cloud, reforzando conceptos clave de **DevOps**, **despliegue por capas** y **operación de aplicaciones contenerizadas en AWS**.

#### Actividades

##### Paso 1 - Implementando la VPC

###### Paso 1.1 - Crear la VPC

- Ve a `VPC` → `Sus VPC` → `Crear VPC`
- Nombre: `devops-vpc`
- IPv4 CIDR: `10.0.0.0/16`
- Tenencia: `Predeterminado`
- Una vez creada: `Acciones` → `Editar la configuración de VPC`:
- Activa `Habilitar la resolución de DNS` y `Habilitar nombres de host DNS`

###### Paso 1.2 - Crear subredes

**Subred pública:**

- Ve a `VPC` → `Subredes` → `Crear subred`
- ID de la VPC: `devops-vpc`
- Nombre: `devops-subnet-publica`
- Zona de disponibilidad: `us-east-1a`
- Bloque de CIDR: `10.0.1.0/24`
- Habilita la IP pública: `Acciones` → `Editar la configuración de la subred`
- Marca `Habilitar la asignación automática de la dirección IPv4 pública`
- Presiona `Guardar`

**Subred privada:**

- Ve a `VPC` → `Subredes` → `Crear subred`
- ID de la VPC: `devops-vpc`
- Nombre: `devops-subnet-privada`
- Zona de disponibilidad: `us-east-1a`
- Bloque de CIDR: `10.0.2.0/24`

###### Paso 1.3 - Crear la puerta de enlace de internet

- Ve a `VPC` → `Puertas de enlace de Internet` → `Crear gateway de Internet`
- Etiqueta de nombre: `devops-igw`
- Una vez creada: `Acciones` → `Conectar a la VPC`
- En VPC disponibles, selecciona `devops-vpc` y presiona `Conectar gateway de Internet`

###### Paso 1.4 - Crear la puerta de enlace NAT

- Ve a `VPC` → `Gateways NAT` → `Crear gateway NAT`
- Configura lo siguiente:
  - Nombre: `devops-natgw`
  - Modo de disponibilidad: `Zonal`
  - Subred: `devops-lab-subnet-privada`
  - Tipo de conectividad: Pública
- Clic en `Asignar IP elástica`
- Clic en `Crear gateway NAT`

###### Paso 1.5 - Crear las tablas de enrutamiento

**Tabla de enrutamiento pública:**

- Ve a `VPC` → `Tablas de enrutamiento` → `Crear tabla de enrutamiento`
- Nombre: `devops-rtb-publica`
- VPC: `devops-vpc`
- Modifica las rutas: `Rutas` → `Editar rutas` → `Agregar ruta`
  - Destino `0.0.0.0/0` → Objetivo `Puerta de enlace de Internet` → `devops-igw` → `Guardar cambios`
- Modifica las subredes: `Asociaciones de subredes` → `Asociaciones de subredes explícitas` → `Editar asociaciones de subredes`
  - Selecciona `devops-subnet-publica`
  - Clic en `Guardar asociaciones`

**Tabla de enrutamiento privada:**

- Ve a `VPC` → `Tablas de enrutamiento` → `Crear tabla de enrutamiento`
- Nombre: `devops-rtb-privada`
- VPC: `devops-vpc`
- Modifica las rutas: `Rutas` → `Editar rutas` → `Agregar ruta`
  - Destino `0.0.0.0/0` → Objetivo `Puerta de enlace NAT` → `devops-natgw` → `Guardar cambios`
- Modifica las subredes: `Asociaciones de subredes` → `Asociaciones de subredes explícitas` → `Editar asociaciones de subredes`
  - Selecciona `devops-subnet-privada`
  - Clic en `Guardar asociaciones`

###### Paso 1.6 - Crear los grupos de seguridad

**Grupo de seguridad expuesto a internet (frontend):**

- Ve a `VPC` → `Grupos de seguridad` → `Crear grupo de seguridad`
- Nombre: `devops-sg-frontend`
- Descripción: `SG para frontend`
- VPC: `devops-vpc`
- Reglas de entrada:

  | Tipo                | Protocolo | Intervalo de puertos | Origen                 |
  |:--------------------|:----------|:---------------------|:-----------------------|
  | Todos los ICMP IPv4 | ICMP      | Todo                 | `0.0.0.0/0` (Anywhere) |
  | HTTP                | TCP       | 80                   | `0.0.0.0/0` (Anywhere) |
  | HTTPS               | TCP       | 443                  | `0.0.0.0/0` (Anywhere) |
  | SSH                 | TCP       | 22                   | `0.0.0.0/0` (Anywhere) |

**Grupo de seguridad privado (backend):**

- Ve a `VPC` → `Grupos de seguridad` → `Crear grupo de seguridad`
- Nombre: `devops-sg-backend`
- Descripción: `SG para Backend`
- VPC: `devops-vpc`
- Reglas de entrada:

  | Tipo                | Protocolo | Intervalo de puertos | Origen                 |
  |:--------------------|:----------|:---------------------|:-----------------------|
  | Todos los ICMP IPv4 | ICMP      | Todo                 | `devops-sg-frontend`   |
  | SSH                 | TCP       | 22                   | `devops-sg-frontend`   |
  | TCP Personalizado   | TCP       | 3001                 | `devops-sg-frontend`   |

**Grupo de seguridad privado (datos):**

- Ve a `VPC` → `Grupos de seguridad` → `Crear grupo de seguridad`
- Nombre: `devops-sg-data`
- Descripción: `SG para data`
- VPC: `devops-vpc`
- Reglas de entrada:

  | Tipo                | Protocolo | Intervalo de puertos | Origen                 |
  |:--------------------|:----------|:---------------------|:-----------------------|
  | Todos los ICMP IPv4 | ICMP      | Todo                 | `devops-sg-backend`    |
  | MySQL/Aurora        | TCP       | 3306                 | `devops-sg-backend`    |
  | SSH                 | TCP       | 22                   | `devops-sg-backend`    |

##### Paso 2 - Implementando las instancias EC2

###### Paso 2.1 - Crear par de claves

- Ve a `EC2` → `Red y seguridad` → `Pares de clave` → `Crear par de claves`
- Nombre: `devops-key`
- Tipo de par de claves: `RSA`
- Formato: `pem`
- Clic en `Crear par de claves`

> **Nota:** Al crear el par de claves, se descargará automáticamente el archivo `devops-key-pem`. Guárdalo en un lugar seguro, ya que lo necesitarás para conectarte mediante el túnel SSH.

###### Paso 2.2 - Crear la plantilla de lanzamiento

- Ve a `EC2` → `Instancias` → `Plantillas de lanzamiento` → `Crear plantilla de lanzamiento`
- Nombre: `devops-lt-base`
- Descripción: `Plantilla base para instancias EC2`
- Imágenes de aplicaciones y sistemas operativos: `Amazon Linux`
- Tipo de instancia: `t3-micro`
- Par de claves: `devops-key`
- Detalles avanzados
  - Perfil de instancia de IAM: `LabInstanceProfile`
  - Datos de usuario:

    ```bash
    #!/bin/bash
    yum update -y
    yum install -y amazon-ssm-agent docker git
    systemctl enable amazon-ssm-agent
    systemctl start amazon-ssm-agent
    systemctl enable docker
    systemctl start docker
    ```

###### Paso 2.3 - Lanzar instancias de EC2

**Lanzar la instancia para `frontend`:**

- Ve a `EC2` → `Instancias` → `Instancias` → `Lanzar la instancia desde una plantilla`
- Plantilla de origen: `devops-it-base`
- Subred: `devops-subnet-publica`
- Zona de disponibilidad: `us-east-1a`
- Firewall: `Seleccionar un grupo de seguridad existente` → `devops-sg-frontend`
- Etiqueta de recursos `Agregar nueva etiqueta`:

  | Clave  | Valor                 |
  |:-------|:----------------------|
  | `Name` | `devops-ec2-frontend` |

- Clic en `Lanzar instancia`

**Lanzar la instancia para `backend`:**

- Ve a `EC2` → `Instancias` → `Instancias` → `Lanzar la instancia desde una plantilla`
- Plantilla de origen: `devops-it-base`
- Subred: `devops-subnet-privada`
- Zona de disponibilidad: `us-east-1a`
- Firewall: `Seleccionar un grupo de seguridad existente` → `devops-sg-backend`
- Etiqueta de recursos `Agregar nueva etiqueta`:

  | Clave  | Valor                |
  |:-------|:---------------------|
  | `Name` | `devops-ec2-backend` |

- Clic en `Lanzar instancia`

**Lanzar la instancia para `data`:**

- Ve a `EC2` → `Instancias` → `Instancias` → `Lanzar la instancia desde una plantilla`
- Plantilla de origen: `devops-it-base`
- Subred: `devops-subnet-privada`
- Zona de disponibilidad: `us-east-1a`
- Firewall: `Seleccionar un grupo de seguridad existente` → `devops-sg-data`
- Etiqueta de recursos `Agregar nueva etiqueta`:

  | Clave  | Valor             |
  |:-------|:------------------|
  | `Name` | `devops-ec2-data` |

- Clic en `Lanzar instancia`

###### Paso 2.4 - Conectarse a las instancias

- Selecciona la instancia (`devops-ec2-frontend`, `devops-ec2-backend` o `devops-ec2-data`) usando `SSM Session Manager`
- Ejecuta los siguientes comandos:
  - `sudo su - ec2-user` → Para cambiar al usuario `ec2-user`
  - `sudo usermod -aG docker ec2-user` → Añade el usuario `ec2-user` al grupo de Docker. Esto es útil para no tener que usar `sudo` cada vez que ejecutes un comando de Docker
  - `docker --version` → Verifica la versión de Docker
  - `git --version` → Verifica la versión de Git

  La ejecución debe verse de esta forma:

  ```bash
  sh-5.2$ sudo su - ec2-user
  [ec2-user@ip-10-0-1-65 ~]$ sudo usermod -aG docker ec2-user
  [ec2-user@ip-10-0-1-65 ~]$ docker --version
  Docker version 25.0.14, build 0bab007
  [ec2-user@ip-10-0-1-65 ~]$ git --version
  git version 2.50.1
  ```

##### Paso 3 -Desplegar la aplicación

###### Paso 3.1 - Copiar los archivos

- Usando `CloudShell` → `Acciones` → `Cargar archivo`
- Carga el archivo `2.3.3 APP tienda-perritos_EC2.zip`
- Ejecuta los siguientes comandos:

  ```bash
  # Descomprimir el archivo
  unzip '2.3.3 APP tienda-perritos_EC2.zip'

  # Crear el bucket
  BUCKET="devops-bucket-$(aws sts get-caller-identity --query Account --output text)"
  aws s3 mb s3://$BUCKET

  # Copiar los archivos al bucket
  aws s3 cp ./tienda-perritos_EC2 s3://$BUCKET/tienda-perritos_EC2 --recursive
  ```

###### Paso 3.2 - Desplegar la base de datos

- Conéctate a la instancia `devops-ec2-data` y cambia al usuario `ec2-user`
- Descarga los archivos desde el Bucket

  ```bash
  BUCKET="devops-bucket-$(aws sts get-caller-identity --query Account --output text)"

  aws s3 cp s3://$BUCKET/tienda-perritos_EC2/db db/ --recursive
  ```

- Ingresa a la carpeta `db`

  ```bash
  cd db/
  ```

- Construye la imagen

  ```bash
  docker build -t tienda-db .
  ```

- Ejecuta la imagen

  ```bash
  docker run -d \
      --name tienda-db \
      -p 3306:3306 \
      -v dbdata:/var/lib/mysql \
      tienda-db
  ```

- Comprueba los logs de la base de datos

  ```bash
  docker logs tienda-db
  ```

###### Paso 3.3 - Desplegar el backend

- Conéctate a la instancia `devops-ec2-backend` y cambia al usuario `ec2-user`
- Descarga los archivos desde el Bucket

  ```bash
  BUCKET="devops-bucket-$(aws sts get-caller-identity --query Account --output text)"

  aws s3 cp s3://$BUCKET/tienda-perritos_EC2/backend backend/ --recursive
  ```

- Ingresa a la carpeta `backend`

  ```bash
  cd backend/
  ```

- Construye la imagen

  ```bash
  docker build -t tienda-backend .
  ```

- Ejecuta la imagen

  ```bash
  docker run -d \
      --name tienda-backend \
      -p 3001:3001 \
      -e DB_HOST=<ip-devops-ec2-data> \
      -e DB_USER=root \
      -e DB_PASSWORD=admin123 \
      -e DB_NAME=tienda_perritos \
      -e DB_PORT=3306 \
      tienda-backend
  ```

  > **Nota:** Reemplaza `<ip-devops-ec2-data>` por la dirección IP de la instancia `devops-ec2-data`. Por ejemplo: `10.0.2.203`.

- Comprueba los logs del backend

  ```bash
  docker logs tienda-backend
  ```

###### Paso 3.4 - Desplegar el frontend

- Conéctate a la instancia `devops-ec2-frontend` y cambia al usuario `ec2-user`
- Descarga los archivos desde el Bucket

  ```bash
  BUCKET="devops-bucket-$(aws sts get-caller-identity --query Account --output text)"

  aws s3 cp s3://$BUCKET/tienda-perritos_EC2/frontend frontend/ --recursive
  ```

- Ingresa a la carpeta `frontend`

  ```bash
  cd frontend/
  ```

- Modifica el contenido del archivo `default.conf`. Reemplaza el valor de `IP_PRIVADA_BACKEND` por la dirección IP de la instancia `devops-ec2-backend`; por ejemplo: `10.0.2.138`

  ```plaintext
  server {
      listen 80;
      server_name _;
      charset utf-8;

      root /usr/share/nginx/html;
      index index.html;

      # Archivos estáticos (tu SPA sencilla)
      location / {
          try_files $uri $uri/ /index.html;
      }

      # Proxy hacia el backend en la subred privada
      location /api/ {
          proxy_pass http://10.0.2.138:3001;
          # EJEMPLO: proxy_pass http://10.0.2.50:3001;

          proxy_set_header Host $host;
          proxy_set_header X-Real-IP $remote_addr;
          proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      }
  }
  ```

- Construye la imagen

  ```bash
  docker build -t tienda-frontend .
  ```

- Ejecuta la imagen

  ```bash
  docker run -d \
      --name tienda-frontend \
      -p 80:80 \
      tienda-frontend
  ```

- Comprueba los logs del frontend

  ```bash
  docker logs tienda-frontend
  ```

- Comprueba el funcionamiento accediendo a la IP pública de la instancia `devops-ec2-frontend`. Por ejemplo: `http://98.80.186.230/`

### Actividad 2.4.2 - Gestión y versionado de imágenes en Amazon ECR y Docker Hub

#### Enunciado

En esta actividad vas a publicar imágenes Docker de una aplicación multicapa (frontend, backend y base de datos) en repositorios de contenedores públicos y privados, utilizando Docker Hub y Amazon Elastic Container Registry (ECR).
Crearás los repositorios, te autenticarás en cada plataforma, etiquetarás correctamente las imágenes Docker y las publicarás mediante comandos `docker push`, para luego validar la existencia de las imágenes y sus versiones (v1, v2 o latest) desde las consolas de Docker Hub y AWS ECR.
Con esta práctica entenderás el rol de los container registries dentro del ciclo DevOps, la diferencia entre repositorios públicos y privados, y la importancia del versionado de imágenes para procesos de despliegue, automatización y pipelines CI/CD en entornos cloud.

#### Actividades

##### Actividad 1 - Publicación de imágenes en Docker Hub

Para la solución vamos a utilizar las instancias EC2 creadas en [Actividad 2.3.2](#actividad-232---desplegar-la-app-web-en-docker-y-en-cada-ec2-por-capas).

###### Paso 1 - Crear cuenta en Docker Hub

1. Ve a [Docker Hub](https://hub.docker.com/)
2. Presiona el botón `Sign up`
3. Selecciona la opción `Personal`
4. Rellena el formulario usando tu correo institucional. Por ejemplo:
   - Email: `john.doe@duocuc.cl`
   - Username: `johnnyduoc`
   - Password: `P@$$w0rd..`

###### Paso 2 - Desplegar la imagen de `tienda-db`

1. Ingresa a la instancia `devops-ec2-data` mediante **SSM**
2. Cambia al usuario `ec2-user`:

   ```bash
   sudo su - ec2-user
   ```

3. Navega a la carpeta `db`:

   ```bash
   cd db/
   ```

4. Inicia sesión en **Docker Hub** usando las credenciales creadas en el [paso 1](#paso-1---crear-cuenta-en-docker-hub):

   ```bash
   docker login
   ```

5. Etiqueta la imagen para el repositorio:

   ```bash
   docker tag tienda-db <usuario_docker_hub>/tienda-db:v1
   ```

   > **Nota:** Reemplaza `<usuario_docker_hub>` con el nombre de usuario creado en el [paso 1](#paso-1---crear-cuenta-en-docker-hub)

6. Publica la imagen en **Docker Hub**

   ```bash
   docker push usuario_docker_hub/tienda-db:v1
   ```

   > **Nota:** Reemplaza `<usuario_docker_hub>` con el nombre de usuario creado en el [paso 1](#paso-1---crear-cuenta-en-docker-hub)

###### Paso 3 - Desplegar la imagen de `tienda-backend`

1. Ingresa a la instancia `devops-ec2-backend` mediante **SSM**
2. Cambia al usuario `ec2-user`:

   ```bash
   sudo su - ec2-user
   ```

3. Navega a la carpeta `backend`:

   ```bash
   cd backend/
   ```

4. Inicia sesión en **Docker Hub** usando las credenciales creadas en el [paso 1](#paso-1---crear-cuenta-en-docker-hub):

   ```bash
   docker login
   ```

5. Etiqueta la imagen para el repositorio:

   ```bash
   docker tag tienda-db <usuario_docker_hub>/tienda-backend:v1
   ```

   > **Nota:** Reemplaza `<usuario_docker_hub>` con el nombre de usuario creado en el [paso 1](#paso-1---crear-cuenta-en-docker-hub)

6. Publica la imagen en **Docker Hub**

   ```bash
   docker push usuario_docker_hub/tienda-backend:v1
   ```

   > **Nota:** Reemplaza `<usuario_docker_hub>` con el nombre de usuario creado en el [paso 1](#paso-1---crear-cuenta-en-docker-hub)

###### Paso 3 - Desplegar la imagen de `tienda-frontend`

1. Ingresa a la instancia `devops-ec2-frontend` mediante **SSM**
2. Cambia al usuario `ec2-user`:

   ```bash
   sudo su - ec2-user
   ```

3. Navega a la carpeta `frontend`:

   ```bash
   cd frontend/
   ```

4. Inicia sesión en **Docker Hub** usando las credenciales creadas en el [paso 1](#paso-1---crear-cuenta-en-docker-hub):

   ```bash
   docker login
   ```

5. Etiqueta la imagen para el repositorio:

   ```bash
   docker tag tienda-db <usuario_docker_hub>/tienda-frontend:v1
   ```

   > **Nota:** Reemplaza `<usuario_docker_hub>` con el nombre de usuario creado en el [paso 1](#paso-1---crear-cuenta-en-docker-hub)

6. Publica la imagen en **Docker Hub**

   ```bash
   docker push usuario_docker_hub/tienda-frontend:v1
   ```

   > **Nota:** Reemplaza `<usuario_docker_hub>` con el nombre de usuario creado en el [paso 1](#paso-1---crear-cuenta-en-docker-hub)

###### Paso 5 - Verificar

1. Accede a los repositorios de [Docker Hub](https://hub.docker.com/repositorios) usando las credenciales creadas en el [paso 1](#paso-1---crear-cuenta-en-docker-hub)
2. Ve a la sección **My Hub**
3. Comprueba la existencia de los repositorios:
   - `tienda-db` con el tag `v1`
   - `tienda-backend` con el tag `v1`
   - `tienda-frontend` con el tag `v1`

##### Actividad 2 - Publicación de imágenes en Amazon ECR

###### Paso 1 - Crear los repositorios en ECR

**Desde la consola AWS:**

1. Ve a `Elastic Container Registry` → `Registro Privado` → `Repositorios`
2. Crea los siguientes repositorios:

   | Nombre            | Mutabilidad | Cifrado   |
   |:------------------|:------------|:----------|
   | `tienda-db`       | `Mutable`   | `AES-256` |
   | `tienda-backend`  | `Mutable`   | `AES-256` |
   | `tienda-frontend` | `Mutable`   | `AES-256` |

**Usando `CloudShell`:**

Ejecuta los siguientes comandos:

```bash
# Para crear el repositorio
aws ecr create-repository \
  --repository-name tienda-db \
  --image-tag-mutability MUTABLE \
  --encryption-configuration encryptionType=AES256 \
  --region us-east-1

# Para crear el repositorio
aws ecr create-repository \
  --repository-name tienda-backend \
  --image-tag-mutability MUTABLE \
  --encryption-configuration encryptionType=AES256 \
  --region us-east-1


# Para crear el repositorio
aws ecr create-repository \
  --repository-name tienda-frontend \
  --image-tag-mutability MUTABLE \
  --encryption-configuration encryptionType=AES256 \
  --region us-east-1
```

###### Paso 2 - Autenticarse desde Docker a ECR

**Publicar la imagen `tienda-db`:**

1. Ingresa a la **instancia EC2** `devops-ec2-data` usando **SSM** y cambia al usuario `ec2-user`
2. Navega a la carpeta `db`:

   ```bash
   cd db/
   ```

3. Ve a `Elastic Container Registry` → `Registro Privado` → `Repositorios`
4. Desde el listado de repositorios privados, selecciona el repositorio `tienda-db`
5. Haz clic en `Ver comandos de envío`
6. Copia el comando para recuperar el token y ejecútalo en la instancia EC2 `devops-ec2-data`. Por ejemplo:

   ```bash
   aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <account_id>.dkr.ecr.us-east-1.amazonaws.com
   ```

7. Crea la imagen `tienda-db`. Por ejemplo:

   ```bash
   docker build -t tienda-db .
   ```

8. Etiqueta la imagen `tienda-db`. Por ejemplo:

   ```bash
   docker tag tienda-db:latest <account_id>.dkr.ecr.us-east-1.amazonaws.com/tienda-db:latest
   ```

9. Publica la imagen `tienda-db`. Por ejemplo:

   ```bash
   docker push <account_id>.dkr.ecr.us-east-1.amazonaws.com/tienda-db:latest
   ```

**Publicar la imagen `tienda-backend`:**

1. Ingresa a la **instancia EC2** `devops-ec2-backend` usando **SSM** y cambia al usuario `ec2-user`
2. Navega a la carpeta `backend`:

   ```bash
   cd backend/
   ```

3. Ve a `Elastic Container Registry` → `Registro Privado` → `Repositorios`
4. Desde el listado de repositorios privados, selecciona el repositorio `tienda-backend`
5. Haz clic en `Ver comandos de envío`
6. Copia el comando para recuperar el token y ejecútalo en la instancia EC2 `devops-ec2-backend`. Por ejemplo:

   ```bash
   aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <account_id>.dkr.ecr.us-east-1.amazonaws.com
   ```

7. Crea la imagen `tienda-backend`. Por ejemplo:

   ```bash
   docker build -t tienda-backend .
   ```

8. Etiqueta la imagen `tienda-backend`. Por ejemplo:

   ```bash
   docker tag tienda-backend:latest <account_id>.dkr.ecr.us-east-1.amazonaws.com/tienda-backend:latest
   ```

9. Publica la imagen `tienda-backend`. Por ejemplo:

   ```bash
   docker push <account_id>.dkr.ecr.us-east-1.amazonaws.com/tienda-backend:latest
   ```

**Publicar la imagen `tienda-frontend`:**

1. Ingresa a la **instancia EC2** `devops-ec2-frontend` usando **SSM** y cambia al usuario `ec2-user`
2. Navega a la carpeta `frontend`:

   ```bash
   cd frontend/
   ```

3. Ve a `Elastic Container Registry` → `Registro Privado` → `Repositorios`
4. Desde el listado de repositorios privados, selecciona el repositorio `tienda-frontend`
5. Haz clic en `Ver comandos de envío`
6. Copia el comando para recuperar el token y ejecútalo en la instancia EC2 `devops-ec2-frontend`. Por ejemplo:

   ```bash
   aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <account_id>.dkr.ecr.us-east-1.amazonaws.com
   ```

7. Crea la imagen `tienda-frontend`. Por ejemplo:

   ```bash
   docker build -t tienda-frontend .
   ```

8. Etiqueta la imagen `tienda-frontend`. Por ejemplo:

   ```bash
   docker tag tienda-frontend:latest <account_id>.dkr.ecr.us-east-1.amazonaws.com/tienda-frontend:latest
   ```

9. Publica la imagen `tienda-frontend`. Por ejemplo:

   ```bash
   docker push <account_id>.dkr.ecr.us-east-1.amazonaws.com/tienda-frontend:latest
   ```

### Actividad 2.5.2 - Construye y despliegue pipeline CICD con GitHub Actions

#### Enunciado

En esta actividad vas a **diseñar, configurar y ejecutar un pipeline de Integración y Entrega Continua (CI/CD)** utilizando **GitHub Actions**, automatizando el proceso de **build**, **publicación** y **despliegue** de una aplicación contenerizada por capas (frontend, backend y base de datos).

Para ello, crearás un **repositorio en GitHub**, definirás **workflows CI/CD independientes por cada capa**, configurarás **secretos y variables de entorno** para una integración segura con servicios de AWS, y ejecutarás el **pipeline ante cambios en el código fuente**.

Como parte del proceso, el pipeline deberá **construir nuevas imágenes Docker**, **publicarlas en Amazon ECR** y **desplegarlas automáticamente en instancias EC2** mediante **AWS Systems Manager (SSM)**, reemplazando los contenedores existentes sin intervención manual. Finalmente, deberás **validar el despliegue**, comprobando que la aplicación se actualiza correctamente tras un cambio en el repositorio.

Esta actividad te permitirá entender de forma práctica cómo los pipelines CI/CD facilitan la **automatización, trazabilidad y repetibilidad** de los despliegues, consolidando el uso de **GitHub Actions**, **Docker**, **ECR** y **EC2** dentro de un flujo DevOps real.

#### Actividades

##### Paso 1 - Preparar el repositorio en GitHub

1. Accede al AVA, descarga el archivo `2.5.3 APP tienda-perritos_Docker_y_Workflow_GITHUB.zip` y descomprímelo.
2. Accede a [GitHub](https://github.com)
3. Inicia sesión con una cuenta existente. Si no tienes una, créala usando tu correo institucional. Por ejemplo:
   - Email: `john.doe@duocuc.cl`
   - Password: `P@$$w0rd..`
   - Username: `johnnyduoc`
4. Crea un repositorio llamado `tienda-perritos`, y sigue las instrucciones para configurarlo
5. Sube las carpetas `db`, `frontend` y `backend` al repositorio

##### Paso 2 - Configurar secretos en GitHub (Settings → Secrets and variables → Actions)

1. En el repositorio, ve a `Settings` → `Security and quality` → `Secrets and variables` → `Actions` → `Repository secrets` → `New repository secret`
2. Crea los siguientes secretos:

   | Name                       | Secret                                                            |
   |:---------------------------|:------------------------------------------------------------------|
   | `AWS_ACCESS_KEY_ID`        | Obtener desde `AWS Details` → `AWS CLI` del módulo de AWS Academy |
   | `AWS_SECRET_ACCESS_KEY`    | Obtener desde `AWS Details` → `AWS CLI` del módulo de AWS Academy |
   | `AWS_SESSION_TOKEN`        | Obtener desde `AWS Details` → `AWS CLI` del módulo de AWS Academy |
   | `AWS_REGION`               | `es-east-1`                                                       |
   | `ECR_REPO_URL_DB`          | `<account_id>.dkr.ecr.us-east-1.amazonaws.com/tienda-db`          |
   | `ECR_REPO_URL_BACKEND`     | `<account_id>.dkr.ecr.us-east-1.amazonaws.com/tienda-backend`     |
   | `ECR_REPO_URL_FRONTEND`    | `<account_id>.dkr.ecr.us-east-1.amazonaws.com/tienda-frontend`    |
   | `ECR_REGISTRY`             | `<account_id>.dkr.ecr.us-east-1.amazonaws.com`                    |
   | `EC2_DB_INSTANCE_ID`       | Obtener desde el listado de instancias EC2                        |
   | `EC2_BACKEND_INSTANCE_ID`  | Obtener desde el listado de instancias EC2                        |
   | `EC2_FRONTEND_INSTANCE_ID` | Obtener desde el listado de instancias EC2                        |

   > **Notas:**
   > 1. Como estamos trabajando con **AWS Academy**, las credenciales que se obtienen desde **AWS Details**, luego **AWS Cli** y dar clic **Show**. Los valores que necesitamos son `aws_access_key_id`, `aws_secret_access_key` y `aws_session_token`. Hay que tener precaución, porque expiran después de 3 o 4 horas de laboratorio. Si da error de credenciales, se deben revisar nuevamente las credenciales que refrezcó AWS.
   >
   > 2. Reemplaza el valor de `<account_id>` con el ID de la cuenta sin guiones. Por ejemplo, si el ID de la cuenta es `2943-6308-7994` el valor de `ECR_REGISTRY` será `294363087994.dkr.ecr.us-east-1.amazonaws.com`.

3. Crea los flujos de trabajo en `Actions` → `set up a workflow yourself`
   - `cicd-tienda-db.yml` → Copia el contenido del archivo `workflows/cicd-tienda-db.yml` → `Commit changes...`
   - `cicd-tienda-backend.yml` → Copia el contenido del archivo `workflows/cicd-tienda-backend.yml` → `Commit changes...`
   - `cicd-tienda-frontend.yml` → Copia el contenido del archivo `workflows/cicd-tienda-frontend.yml` → `Commit changes...`
  
##### Paso 3 - Ejecutar la práctica en clase

1. En la carpeta `db`, reemplaza el contenido del archivo `init.sql` con el siguiente código:

   ```sql
   CREATE DATABASE IF NOT EXISTS tienda_perritos;
   USE tienda_perritos;

   CREATE TABLE IF NOT EXISTS productos (
       id INT AUTO_INCREMENT PRIMARY KEY,
       nombre VARCHAR(100) NOT NULL,
       descripcion VARCHAR(255),
       precio DECIMAL(10,2) NOT NULL,
       stock INT NOT NULL
   );

   INSERT INTO productos (nombre, descripcion, precio, stock) VALUES
   ('Alimento Cachorro Premium', 'Sabor pollo, razas pequeñas', 19990, 15),
   ('Alimento Adulto Light', 'Control de peso, razas medianas', 17990, 8),
   ('Snacks Dentales', 'Ayuda a la limpieza dental', 5990, 30),
   ('Alimento Adulto Pedigree', 'Sabor carne', 15990, 40),
   ('Bravery pollo Adulto raza pequena', 'Sabor a pollo', 25990, 20);
   ```

2. En la carpeta `backend`, reemplaza el contenido del archivo `server-js` de la siguiente forma:

   ```javascript
   // Endpoint de salud
   app.get("/api/health", (req, res) => {
     res.json({ status: "ok", message: "Backend de tienda de perritos en ejecución." });
   });
   ```

   Por:

   ```javascript
   // Endpoint de salud
   app.get("/api/health", (req, res) => {
     res.json({ status: "ok", message: "🚀 Backend de tienda de perritos en ejecución." });
   });
   ```

3. En la carpeta `frontend`, reemplaza el contenido del archivo `index.html` de la siguiente forma:

   ```html
   <title>Tienda de Alimentos para Perritos &#128054;</title>
   ```

   Por:

   ```html
   <title>Tienda de Alimentos para Perritos 🐶</title>
   ```

4. Envía los cambios a GitHub:

   ```bash
   git add .
   git commit -m "Prueba de despliegue automático"
   git push -u origin main
   ```

##### Paso 4 - Validar el despliegue en AWS

En las instancias EC2, ejecuta los siguientes comandos:

```bash
# Para verificar que los contenedores se encuentren activos
docker ps

# Revisar los posibles errores
docker logs

# SOLO DESDE devops-ec2-data
docker exec -it tienda-db mysql -u root -padmin123 -e "USE tienda_perritos; SELECT * FROM productos;"
```

Desde el navegador, accede a la IP pública de la instancia EC2 `devops-ec2-frontend` y verifica que la aplicación carga, el backend responde y los productos cargados en la base de datos se muestran correctamente.

## Anexo

### Comandos útiles de Docker

```bash
# Conocer el estado del servicio Docker
sudo systemctl status docker

# Muestra información detallada del sistema
sudo docker info

# Listar imágenes locales
docker images

# Descargar imagen
docker pull <nombre_de_imagen>

# Eliminar imagen
docker rmi <nombre_de_imagen>

# Eliminar imágenes sin usar
docker image prune

# Construir desde Dockerfile
docker build -t <nombre_de_imagen>:<version> .

# Etiquetar imagen
docker tag <nombre_de_imagen>:<version> <nombre_de_imagen>:<tag>

# Ejecutar contenedor
docker run <nombre_de_imagen>

# Ejecutar contenedor en segundo plano (detached)
docker run -d <nombre_de_imagen>

# Ejecutar contenedor con varias opciones
docker run -d \
    --name <nombre_contenedor> \
    -p <puerto_host>:<puerto_contenedor> \
    -e <variable>=<valor>
    <nombre_de_imagen>

# Iniciar un contenedor detenido
docker start <nombre_contenedor>

# Detener un contenedor detenido
docker stop <nombre_contenedor>

# Forzar detención de un contenedor
docker kill <nombre_contenedor>

# Reiniciar un contenedor detenido
docker restart <nombre_contenedor>

# Eliminar un contenedor detenido
docker rm <nombre_contenedor>

# Forzar eliminación aunque esté corriendo
docker rm -f <nombre_contenedor>

# Listar contenedores corriendo
docker ps

# Listar todos los cotenedores
docker ps -a

# Ver logs del contenedor
docker logs <nombre_contenedor>

# Ver logs en tiempo real
docker logs -f <nombre_contenedor>

# Uso de CPU/Memoria en tiempo real
docker stats
```

Por ejemplo, si queremos ejecutar la imagen de `MySQL 8` debemos realizar los siguientes pasos:

```bash
# Descarga la imagen de MySQL
docker pull mysql:8.4.9-oracle

# Inicia la imagen de MySQL
docker run -d \
    --name mysql:8.4.9-oracle \
    -p 3306:3306 \
    -e MYSQL_ROOT_PASSWORD=password \
    mysql:8.0
```

### Comandos útiles de Docker Compose

```bash
# Levantar servicios
docker compose up

# Levantar servicios en segundo plano
docker compose up -d

# Reconstruir las imágenes antes de levantar
docker compose up --build

# Detener y eliminar contenedores
docker compose down

# Detener y eliminar contenedores y sus volúmenes
docker compose down -v

# Estado de los servicios
docker compose ps

# Logs en tiempo real
docker compose logs -f

# Reiniciar un servicio específico
docker compose restart <nombre_servicio>
```

Por ejemplo, si tenemos el siguiente archivo `compose.yaml`...

```yaml
services:
  mysql:
    image: mysql:8.4.9-oracle
    environment:
      MYSQL_ROOT_PASSWORD: password
    ports:
      - "3306:3306"
    volumes:
      - db_data:/var/lib/mysql
  
  phpmyadmin:
    image: phpmyadmin:latest
    environment:
      PMA_HOST: mysql
      PMA_PORT: 3306
    ports:
      - "8080:80"
    depends_on:
      - mysql

volumes:
  db_data:
```

... podemos ejecutar los siguientes comandos:

```bash
# Levantar los servicios en segundo plano
docker compose up -d

# Reiniciar el servicio `phpmyadmin`
docker compose restart phpmyadmin

# Detener y eliminar los contenedores y los volúmenes
docker compose down -v
```

## Autor

José Miguel Candia - [GitHub](https://github.com/jmcandia) | [Correo](mailto:jo.candiah@profesor.duoc.cl)

## Licencia

[MIT](https://choosealicense.com/licenses/mit/)

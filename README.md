# Prospects Frontend

Aplicación web desarrollada con Vue 3 y Quasar para la captura y registro de prospectos mediante la API de Prospects.

## Instalación

### 1. Clonar el repositorio

```bash
git clone https://github.com/rocouv/prospects_frontend.git
cd prospects_frontend
```

### 2. Instalar las dependencias

```bash
npm install
```

### 3. Configurar las variables de entorno

Crear el archivo `.env` a partir de `.env.example`.
```bash
cp .env.example .env
```
El archivo `.env` debe contener la URL del backend:

```env
QCLI_API_URL=http://localhost:8000/api
```

## Ejecución

Antes de iniciar el frontend, asegúrate de que el backend Laravel esté disponible en:

```text
http://localhost:8000
```

Después inicia Quasar:

```bash
quasar dev
```

La aplicación estará disponible normalmente en:

```text
http://localhost:9000
```

```env
QCLI_API_URL=http://localhost:8000/api
```

## Configuración de desarrollo

El frontend consume por defecto la API en:

```text
http://localhost:8000/api
```

Si el backend utiliza otro host o puerto se modifica:

```env
QCLI_API_URL=http://localhost:8000/api
```

y reinicia Quasar.
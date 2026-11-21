# Laboratorio: Node.js + MongoDB + EJS

**Estudiantes:** esneider gonzalez 
**Fecha:** 04/09/26

Aplicación sencilla en Node.js que se conecta a MongoDB Atlas, obtiene un listado de personas y las muestra en una tabla HTML usando el motor de plantillas EJS.

---

## Qué hace el proyecto

1. Se conecta a la base de datos `lab_personas` en MongoDB Atlas.
2. Lee todos los documentos de la colección `personas`.
3. Los ordena por apellido y nombre.
4. Los muestra en una tabla en el navegador (`http://localhost:3000`).

Flujo aproximado:

```
Navegador → server.js → Express (src/index.js)
                      → rutas → controlador → modelo Persona
                      → consulta MongoDB
                      → vista EJS (index.ejs) → HTML
```

---

## Tecnologías

| Paquete   | Para qué se usa                          |
|-----------|------------------------------------------|
| express   | Servidor web                             |
| mongoose  | Conexión y consultas a MongoDB           |
| ejs       | Plantillas HTML con datos dinámicos      |
| dotenv    | Leer variables de entorno desde `.env`   |

---

## Cómo levantarlo

### 1. Requisitos

- Node.js 20.19 o superior
- Una base en MongoDB Atlas con la colección `personas`

### 2. Instalar dependencias

```bash
npm install
```

### 3. Configurar el `.env`

Copia el ejemplo:

```bash
# Windows
copy .env-example .env

# Mac / Linux
cp .env-example .env
```

Edita `.env` y pon tu URL de Atlas (con la base `lab_personas`):

```env
PORT=3000
MONGODB_URI=mongodb+srv://USUARIO:CLAVE@cluster.xxxxx.mongodb.net/lab_personas?retryWrites=true&w=majority
```

> Importante: la URL debe empezar por `mongodb+srv://` y llevar `/lab_personas` antes del `?`.

### 4. Datos de prueba en Atlas

En la colección `personas` de la base `lab_personas` inserta documentos con esta forma:

```json
{ "nombre": "Ana", "apellido": "Gómez", "edad": 21, "correo": "ana.gomez@correo.com" }
```

Ejemplos usados en el lab:

- Ana Gómez, 21, ana.gomez@correo.com  
- Luis Pérez, 23, luis.perez@correo.com  
- María Rojas, 19, maria.rojas@correo.com  
- Camilo Herrera, 22, camilo.herrera@correo.com  

### 5. Arrancar

```bash
npm run dev
```

o

```bash
npm start
```

En la consola deberías ver algo así:

```
Conectado a MongoDB. Base de datos: lab_personas
Servidor listo en http://localhost:3000
```

Abre el navegador en **http://localhost:3000**.

---

## Estructura del proyecto

```
lab-node-db/
├── server.js                 # Arranca la app y el puerto
├── .env                      # URL de Atlas (no se sube a Git)
├── .env-example
├── package.json
├── public/
│   └── css/estilos.css
└── src/
    ├── index.js              # Config de Express + rutas
    ├── configurations/
    │   └── database.js       # Conexión a MongoDB
    ├── models/
    │   └── persona.js        # Esquema de Persona
    ├── controllers/
    │   └── personaController.js
    ├── routes/
    │   └── personaRoutes.js
    └── views/
        └── personas/
            └── index.ejs     # Tabla HTML
```

### Resumen de cada parte

- **database.js** → conecta con `mongoose.connect` usando `MONGODB_URI`.
- **persona.js** → define el esquema (nombre, apellido, edad, correo) y el modelo.
- **personaController.js** → hace `Persona.find()`, ordena y manda los datos a la vista.
- **personaRoutes.js** → asocia `GET /` con el controlador.
- **index.js** → configura EJS, archivos estáticos y monta las rutas.
- **index.ejs** → recorre el arreglo `personas` y pinta la tabla.

---

## Si algo falla

| Problema | Qué revisar |
|----------|-------------|
| `Cannot find module 'dotenv'` / `ejs` | Ejecutar `npm install` |
| `Falta MONGODB_URI` | Crear el `.env` y pegar la URL de Atlas |
| `bad auth` / Authentication failed | Usuario o contraseña incorrectos en la URI |
| `IP that isn't whitelisted` | En Atlas → Network Access, autorizar tu IP (o `0.0.0.0/0`) |
| Tabla vacía | Revisar que la base se llame `lab_personas` y la colección `personas` |
| Celdas vacías | Los nombres de los campos en Mongo y en el esquema deben coincidir (`nombre`, `apellido`, etc.) |

---

## Entrega

El código va en un repositorio propio de GitHub. El archivo `.env` no se sube (está en `.gitignore`).

```bash
git remote set-url origin https://github.com/TU_USUARIO/lab-node-db.git
git add .
git commit -m "Completa el laboratorio de listado de personas"
git push -u origin HEAD
```

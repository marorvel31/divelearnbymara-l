# 🌎 DiveLearn By Mar - Installation and Deployment Guide
# 🌎 DiveLearn By Mar - Guía de Instalación y Despliegue

---

## 📖 Description | Descripción
DiveLearn By Mar is a web platform where users can learn about various topics such as **technology, diving, cooking, first aid, and more**. This project can be executed **locally** or **deployed on AWS**.

DiveLearn By Mar es una plataforma web donde los usuarios pueden aprender sobre diversos temas como **tecnología, buceo, cocina, primeros auxilios y más**. Este proyecto puede ejecutarse **de manera local** o **desplegarse en AWS**.

---
📝 Key Files Explanation | Explicación de Archivos Clave

📌 contacts.js (Contact Management | Gestión de Contactos)

This script handles the interaction with the API, allowing loading, adding, and displaying contacts.
Este script maneja la interacción con la API, permitiendo cargar, agregar y mostrar contactos.

🔹 Key Functions | Funciones Clave

      fetchContacts() → Fetches the list of contacts from the API.|Obtiene la lista de contactos desde la API.
      renderContacts(data) → Displays the contacts in the HTML table. | Muestra los contactos en la tabla HTML.
      addContact(event) → Sends a new contact to the API. | Envía un nuevo contacto a la API.
---

# 🚀 Installation and Execution Guide | Guía de Instalación y Ejecución

## 🔹 1️⃣ Run Locally | Ejecutar en Local

### 📌 Requirements | Requisitos
Before running the project, make sure you have installed:
- **Git** → [Download | Descargar](https://git-scm.com/downloads)
- **Node.js & npm** → [Download | Descargar](https://nodejs.org/)
- **A web browser** (Chrome, Firefox, Edge)
- **Optional | Opcional**: A local server such as Live Server in VS Code.

Antes de ejecutar el proyecto, asegúrate de tener instalado:
- **Git** → [Descargar](https://git-scm.com/downloads)
- **Node.js y npm** → [Descargar](https://nodejs.org/)
- **Un navegador web** (Chrome, Firefox, Edge)
- **Opcional**: Un servidor local como Live Server en VS Code.

** In case that you will run on aws, choose the folder website_mar_aws in case you will run local choose website_mar_local | ** En caso de que vaya a ejecutar en aws, elija la carpeta website_mar_aws, en caso de que vaya a ejecutar local elija  website_mar_local
---

### 📌 Step-by-Step Installation | Instalación Paso a Paso


1️⃣ **Clone the repository | Clonar el repositorio**
```sh
git clone https://github.com/marorvel31/divelearnbymara-l.git
cd divelearnmara-l
cd website_mar_aws | cd website_mar_local*
Depending on your selection move to the right directory |  Dependiendo de tu selección, muevete al directorio correcto
```

2️⃣ **Install dependencies | Instalar dependencias**
```sh
npm install express cors
```

3️⃣ **Start the Backend Proxy | Iniciar el Servidor Proxy**
```sh
node server.js
```
✅ The proxy will run at `http://localhost:3000`
✅ El proxy se ejecutará en `http://localhost:3000`

4️⃣ **Open the Frontend | Abrir el Frontend**
- Option 1 | Opción 1: Open `index.html` directly in a browser.
- Option 2 | Opción 2: Use **Live Server** in VS Code:
  ```sh
  npx live-server
  ```

5️⃣ **Update the API URL | Actualizar la URL de la API**
Edit `contacts.js` and replace the API URL:
```javascript
const API_URL = "http://localhost:3000/api/contacts";
```

✅ Your website will now load contacts from the local server.
✅ Tu sitio web ahora cargará contactos desde el servidor local.

---

## 🔹 2️⃣ Deploy on AWS | Desplegar en AWS

### 📌 Backend Deployment (AWS Lambda + API Gateway) | Despliegue del Backend (AWS Lambda + API Gateway)

1️⃣ **Deploy AWS Lambda Function | Desplegar la función Lambda**
- Go to **AWS Lambda** → Create a function.
- Select **Node.js 18+** as the runtime.
- Copy & paste the following Lambda function:
  ```javascript
  export const handler = async (event) => {
      const API_URL = "https://sa-tech-assessment.replit.app/api/assessment/contacts";
      try {
          const response = await fetch(API_URL);
          const data = await response.json();
          return {
              statusCode: 200,
              headers: { "Access-Control-Allow-Origin": "*" },
              body: JSON.stringify(data),
          };
      } catch (error) {
          return {
              statusCode: 500,
              headers: { "Access-Control-Allow-Origin": "*" },
              body: JSON.stringify({ error: "Failed to fetch data" }),
          };
      }
  };
  ```
- Deploy the function.

2️⃣ **Create API Gateway | Crear API Gateway**
- Go to **AWS API Gateway** → Create a HTTP API.
- Select **Lambda Proxy Integration**.
- Deploy the API and copy the **Invoke URL**.

3️⃣ **Update the API URL in `contacts.js`**
```javascript
const API_URL = "https://xyz123.execute-api.us-east-1.amazonaws.com/dev/contacts";
```

---

### 📌 Frontend Deployment (AWS S3 + CloudFront) | Despliegue del Frontend (AWS S3 + CloudFront)

1️⃣ **Create an S3 Bucket | Crear un Bucket en S3**
- Go to **AWS S3** → Create a bucket.
- Disable **"Block Public Access"**.
- Enable **Static Website Hosting**.

2️⃣ **Upload Website Files | Subir los archivos**
```sh
aws s3 cp . s3://YOUR_BUCKET_NAME --recursive
```

3️⃣ **Make Bucket Public | Hacer público el bucket**
Edit the **Bucket Policy**:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::YOUR_BUCKET_NAME/*"
    }
  ]
}
```

4️⃣ **Create a CloudFront Distribution | Crear una Distribución CloudFront**
- Select **Origin Domain Name** as your S3 bucket.
- Set **Default Root Object** to `index.html`.
- Enable **Custom SSL Certificate (ACM)** for HTTPS.

5️⃣ **Update Route 53 DNS (Optional) | Actualizar DNS en Route 53 (Opcional)**
- Create an **A record** that points to **CloudFront**.

6️⃣ **Invalidate CloudFront Cache | Invalidar Caché de CloudFront**
```sh
aws cloudfront create-invalidation --distribution-id YOUR_DISTRIBUTION_ID --paths "/*"
```

✅ Your site is now live at `https://your-domain.com`.
✅ Tu sitio está en vivo en `https://tu-dominio.com`.

---

# 📁 Project Structure | Estructura del Proyecto

| 📂 File | 📌 Description | 📌 Descripción |
|--------|--------------|--------------|
| `index.html` | Main page structure. | Estructura principal del sitio. |
| `js/contacts.js` | Handles API interaction. | Maneja la API de contactos. |
| `server.js` | Local proxy server. | Servidor proxy local. |

---

# 🛠 Troubleshooting | Solución de Problemas

| Problem | Solution | Problema | Solución |
|---------|----------|---------|----------|
| Contacts do not load | Check if API Gateway works. | No carga contactos | Verifica que API Gateway funciona. |
| Site not found | Ensure S3 permissions are set. | Sitio no encontrado | Asegura que S3 tiene permisos. |
| HTTPS not working | Validate SSL in CloudFront. | HTTPS no funciona | Valida SSL en CloudFront. |

🚀 **Now your project is ready for AWS!** 🎉  
🚀 **¡Ahora tu proyecto está listo para AWS!** 🎉


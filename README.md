# Conecta Horizontal — Landing Page

Landing page oficial de **Conecta Horizontal**, la plataforma de votación electrónica líder en Colombia para asambleas de propiedad horizontal, conjuntos residenciales y centros comerciales.

Este repositorio contiene la estructura estática del sitio web, la configuración de pruebas de optimización SEO y accesibilidad, y el pipeline de Integración y Despliegue Continuo (CI/CD).

---

## 🚀 Características Técnicas

* **Maquetación y Estilos:** Construido con HTML5 semántico y estilizado de forma híbrida utilizando [Tailwind CSS](https://tailwindcss.com/) (CDN) y Vanilla CSS para efectos personalizados (Glassmorphism, patrones de grilla, animaciones de flotado).
* **SEO y Marcación Semántica:**
  * Metaetiquetas primarias y de geo-targeting optimizadas.
  * Tarjetas Open Graph (Facebook/WhatsApp/LinkedIn) y Twitter Cards configuradas.
  * Datos estructurados estructurados con múltiples scripts JSON-LD de Schema.org (`Organization`, `LocalBusiness`, `WebSite`, `Service` y `FAQPage`).
* **Sección "Trabaja con Nosotros" (Formulario Seguro):**
  * Carga de archivos para hojas de vida en formatos `.pdf`, `.doc` y `.docx` (limite estricto de **5 MB**).
  * **Honeypot:** Campo oculto anti-bots que descarta envíos de spam automatizado de forma silenciosa.
  * **Math Captcha:** Pregunta matemática generada dinámicamente con JavaScript para verificar la interacción de usuarios reales.
  * **Rate Limiting:** Control de frecuencia local mediante `localStorage` que bloquea envíos repetidos del mismo usuario durante 15 minutos.
* **Interactividad y Soporte:** Sección de acordeones animados en JavaScript puro para las Preguntas Frecuentes.

---

## 🧪 Desarrollo Local y Pruebas

Para trabajar en el proyecto localmente y asegurarte de que ningún cambio rompa el SEO o la accesibilidad, sigue estos pasos:

### 1. Instalar dependencias
Asegúrate de contar con Node.js y ejecuta en la raíz del proyecto:
```bash
npm install
```

### 2. Ejecutar pruebas unitarias
El proyecto utiliza [Jest](https://jestjs.io/) y [Cheerio](https://cheerio.js.org/) para validar que la estructura del HTML, los metadatos SEO esenciales, la jerarquía de encabezados, los atributos de accesibilidad (`alt`, `width`, `height`) y la validez de los scripts JSON-LD sigan funcionando perfectamente.
```bash
npm test
```

### 3. Servidor de desarrollo
Puedes levantar un servidor web local para visualizar el sitio en tiempo real ejecutando:
```bash
npx http-server -p 8080
```
Luego, abre en tu navegador: `http://localhost:8080/`

---

## 🚦 Pipeline de CI/CD (GitHub Actions)

El proyecto cuenta con un flujo de automatización configurado en `.github/workflows/ci.yml`.

Cada vez que se realiza un **Pull Request** o un **Push** a cualquier rama, el workflow ejecuta:
1. **Validación de Landing (CI):** Instala las dependencias y ejecuta `npm test`. Si alguna prueba de SEO o accesibilidad falla, el build falla y te notifica.
2. **Despliegue Continuo (CD):** Si las pruebas pasan y el código se subió a la rama principal `main`, se conecta de forma segura a tu instancia en **Oracle Cloud** y copia mediante SCP los archivos de producción (`index.html`, `Logo.png`, `robots.txt`, `sitemap.xml`, etc.) al directorio web público de tu servidor.

### Configuración de Secretos en GitHub
Para que el job de despliegue (`desplegar-landing`) se ejecute correctamente, debes configurar los siguientes secretos en la configuración de tu repositorio de GitHub (**Settings > Secrets and variables > Actions**):

| Secreto | Descripción | Ejemplo |
| :--- | :--- | :--- |
| `SSH_HOST` | Dirección IP pública de tu instancia de Oracle Cloud | `129.146.xx.xx` |
| `SSH_USERNAME` | Nombre de usuario de acceso SSH al servidor | `ubuntu` o `opc` |
| `SSH_KEY` | Clave privada SSH completa (contenido del archivo `.key`/`.pem`) | `-----BEGIN OPENSSH PRIVATE KEY-----...` |
| `REMOTE_TARGET` | Ruta absoluta de la carpeta pública del servidor web | `/var/www/html/` |

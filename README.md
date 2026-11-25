# Cotizaciones Ya! 
**Cotizaciones Ya!** is a Venezuelan platform where businesses register, manage their products and services with prices, and (in a later stage) connect their social media accounts so customers can receive automatic quotes.

## Summary

This section contains the description for **Stage 1: the static frontend only**. 

It is plain HTML, CSS (Bootstrap 5) and SCSS. There is no JavaScript, no backend, 
and no database yet.

- 7 responsive pages, mobile first.
- Bootstrap 5.3 loaded from a CDN. Custom styles live in `public/scss/styles.scss`.
- Comments starting with `AQUI` mark every place where an image, a script or a backend route must be connected.
- Open `index.html` in a browser to see it (internet connection needed for Bootstrap, icons and fonts).

---

## Detailed documentation

### 1. Project status

| Stage | Scope | Status |
|---|---|---|
| 1 | Static frontend (HTML, CSS, SCSS) | **This repository** |
| 2 | Backend API, authentication, database | Planned |
| 3 | Quote engine and social media integration | Planned |

### 2. Tech stack

- **HTML5** with semantic markup and native form validation (`required`, `type="email"`, `pattern`, `minlength`).
- **Bootstrap 5.3.3** (CSS only), loaded from jsDelivr.
- **Bootstrap Icons 1.11.3** and the **Plus Jakarta Sans** font, both imported from the SCSS file.
- **SCSS** compiled to `public/css/styles.css` with [Dart Sass](https://sass-lang.com/dart-sass).

### 3. Folder structure

```
cotizaciones-ya/
├── index.html                Home
├── registro.html             Company registration, step 1
├── registro-catalogo.html    Company registration, step 2 (initial catalog)
├── login.html                Sign in
├── dashboard.html            Business dashboard
├── productos.html            Products and services table (edit / delete)
├── promociones.html          Promotions center
├── README.md
└── public/                   All static assets
    ├── css/styles.css        Compiled from SCSS. Do not edit by hand.
    ├── scss/styles.scss      Source styles. Edit this file.
    ├── js/                   Empty for now (future scripts)
    └── image/                Put hero.jpg and logo.svg here
```

### 4. Running the project

No build step is needed to view it:

1. Place your images in `public/image/` (see section 7).
2. Open `index.html` in a browser, or serve the folder:
   ```bash
   npx serve .
   ```

To change the styles, edit `public/scss/styles.scss` and recompile:

```bash
npm install -g sass
sass public/scss/styles.scss public/css/styles.css
# or, to recompile on every change:
sass --watch public/scss/styles.scss:public/css/styles.css
```

### 5. Pages

| Page | Purpose | Main elements |
|---|---|---|
| `index.html` | Public home | Brand, headline, hero image, "Crear mi empresa" and "Iniciar sesión" buttons |
| `registro.html` | Registration, step 1 of 2 | Company name, RIF / ID, category (products, services or both), email, phone, password, Instagram and Facebook fields with "Conectar" buttons |
| `registro-catalogo.html` | Registration, step 2 of 2 | Product / Service switch, name, brand, manufacturer, service description, price, "Agregar a la lista" button, list of added items |
| `login.html` | Sign in | Email, password, "Recordar mi sesión", forgot-password link, link to registration |
| `dashboard.html` | Main panel | "Activo" status badge, four counters, main menu |
| `productos.html` | Manage the catalog | Collapsible add form, search and type filter, table with Edit and Delete buttons |
| `promociones.html` | Promotions center | Campaign name, season, message, offer, start and end dates, channels (Instagram, Facebook, WhatsApp), preview card |

**Dashboard counters** (sample values in the HTML):

- Products and services: how many items the company currently has.
- Quotes: how many quote requests were made during the month (not purchases).
- Active promotions.
- Potential customers: distinct accounts that requested a quote. It will be filled once the API exists.

### 6. Design system

| Token | Value | Use |
|---|---|---|
| Trust blue | `#2459D3` | Main actions, links |
| Institutional blue | `#173B83` | Titles, brand |
| Success green | `#24B47E` | Active and confirmed states |
| Offer orange | `#F3A847` | Promotions |
| Light gray | `#F4F6FA` | Page background |

- **Font:** Plus Jakarta Sans.
- **Inputs and buttons:** pill-shaped, at least 44 px tall for touch screens.
- **Cards:** white, 1 rem radius, soft shadow (`.app-card`).
- **Colors** are defined as SCSS variables at the top of `styles.scss`. Bootstrap's own classes are overridden through CSS variables, so Bootstrap does not need to be recompiled.

### 7. Where to connect things (the `AQUI` markers)

Search the project for `AQUI` to find every integration point.

| Marker | File(s) | What to do |
|---|---|---|
| `AQUI SE CARGA LA IMAGEN DEL ENCABEZADO` | `index.html` | Add `public/image/hero.jpg` |
| `AQUI SE CARGA EL LOGO` | all pages | Add `public/image/logo.svg` and replace the icon placeholder |
| `AQUI ESCRIBE LA DIRECCION DEL CSS PROPIO` | all pages | Path to `public/css/styles.css` |
| `AQUI SE CARGA EL JAVASCRIPT` | all pages | Add `<script src="public/js/main.js">` |
| `AQUI SE CARGA BOOTSTRAP JS` | all pages | Needed for modals and dropdowns |
| `AQUI ESCRIBE LA DIRECCION DEL BACKEND` | forms | Set the form `action` to the API route |
| `AQUI SE CARGAN LOS NUMEROS` / `LAS FILAS` / `LOS ITEMS` | dashboard, products, step 2 | Fill the page with data from the API |
| `AQUI SE ABRE EL FORMULARIO DE EDICION` | `productos.html` | Open an edit modal (requires Bootstrap JS) |
| `AQUI SE ACTUALIZA LA VISTA PREVIA` | `promociones.html` | Update the preview live as the user types |

**API routes referenced in the comments** (proposed for Stage 2):

| Action | Route |
|---|---|
| Register company (step 1) | `POST /api/auth/register` |
| Sign in | `POST /api/auth/login` |
| Add item ("Agregar a la lista") | `POST /api/items` |
| List items | `GET /api/items` |
| Delete item | `DELETE /api/items/{id}` |
| Dashboard numbers | `GET /api/dashboard/summary` |
| Save promotion | `POST /api/campaigns` |

### 8. Responsive behavior

- **Mobile first.** Forms use one column and full-width buttons.
- **Phones (below 992 px):** the dashboard shows the "Menú principal" list. Inner pages show a "Panel empresarial" link at the top to go back.
- **Desktop (992 px and up):** a fixed sidebar replaces the list, and forms and cards use more columns.
- The products table scrolls sideways inside its own container, so the page never scrolls horizontally.

### 9. Features that work without JavaScript

- The Product / Service switch uses Bootstrap's `btn-check` radio buttons.
- The "Agregar producto o servicio" form on the products page uses the native `<details>` element.
- Required fields, email format, minimum password length and the RIF format are checked by the browser.

### 10. Known limitations

- Nothing is saved: "Agregar a la lista", "Editar", "Eliminar", "Conectar" and "Guardar promoción" do not do anything yet.
- Bootstrap, icons and fonts come from a CDN, so an internet connection is required.
- The "Finalizar registro" and "Continuar más tarde" buttons, the forgot-password page and the season options on the promotions page are placeholders to be confirmed.
- The pages "Gestionar cotizaciones", "Personalizar mensajes", "Datos de mi empresa" and "Configuración" are not designed yet. Their menu links point to `#`.
- Show / hide password and the live promotion preview need JavaScript.
- The pages have not been tested across browsers and devices yet.
- Sass currently prints a deprecation warning for `darken()` in `styles.scss`. It compiles correctly.

### 11. Next steps

1. Design and add the missing pages.
2. Add `public/js/main.js` with form handling, the edit modal and the live preview.
3. Build the backend (Stage 2) and connect the routes listed in section 7.
4. Test on real phones and tablets.

### 12. License

To be defined.

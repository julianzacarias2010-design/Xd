# Lista Bordó A.J.T.T. — sitio interactivo

Sitio estático preparado para GitHub Pages, con Firebase Authentication (Google) y Cloud Firestore para participación.

## Qué incluye

- Diseño responsive para celular y PC.
- Portada, propuestas, participación, transparencia y composición de la lista.
- Buscador y filtros de propuestas.
- Likes por usuario: un apoyo por propuesta y por cuenta.
- Comentarios y aportes clasificados como comentario, sugerencia, solicitud o modificación.
- Envío de nuevas propuestas con estado `pendiente` para revisión.
- Perfil con nombre y foto de Google; el correo no se publica en los comentarios.
- Eliminación de comentarios propios.
- Sección de transparencia con ingresos, gastos y balance.
- Modo claro/oscuro y detalles de accesibilidad.
- Reglas de Firestore incluidas.

## 1. Configurar Firebase

1. Entrá a Firebase Console y creá un proyecto.
2. Registrá una aplicación web y copiá el objeto de configuración que entrega Firebase.
3. Pegá los valores en `firebase-config.js`.
4. En Authentication, habilitá el proveedor **Google**.
5. En Authentication → Settings → Authorized domains, agregá el dominio de la página de GitHub Pages (y cualquier dominio de pruebas que uses).
6. Creá una base de datos Cloud Firestore.
7. Publicá las reglas de `firestore.rules` en Firestore Rules.

Para un sitio publicado en GitHub Pages, no hay que poner claves privadas en este repositorio. La configuración web de Firebase forma parte del cliente; la protección de los datos se realiza con Authentication y Security Rules.

## 2. Convertir a administrador

La escritura de `transparencia` y la moderación de `nuevasPropuestas` están reservadas a admins.

Después de iniciar sesión con la cuenta que administrará el Centro:

1. Copiá el UID de Firebase Authentication de esa cuenta.
2. En Firestore, creá el documento:

`config/admins/TU_UID`

El documento puede tener cualquier campo simple, por ejemplo:

```text
role: admin
```

No hay que publicar el UID en la web.

## 3. Cargar movimientos de transparencia

En la colección `transparencia`, cada documento puede tener:

```text
type: "ingreso" | "gasto"
amount: número
concept: texto breve
date: fecha
note: texto opcional
```

La web calcula automáticamente los totales de ingresos, gastos y balance a partir de los registros publicados.

## 4. Cómo funciona la participación

- La lectura de propuestas, comentarios y registros de transparencia es pública.
- Dar like, comentar/aportar y enviar una propuesta nueva requiere Google Authentication.
- Los likes se guardan como `propuestas/{id}/likes/{uid}`, evitando más de un apoyo por cuenta.
- Los comentarios se guardan en `propuestas/{id}/comentarios`.
- Las nuevas propuestas se guardan en `nuevasPropuestas` con estado `pendiente` y no se publican como propuestas oficiales hasta que sean revisadas.

## 5. Publicar en GitHub Pages

Podés subir estos archivos a la raíz de un repositorio y activar GitHub Pages desde Settings → Pages. Elegí la rama y la carpeta donde están `index.html`, `styles.css` y `app.js`.

No uses un servidor PHP: el sitio está hecho para hosting estático.

## 6. Canales oficiales

En `app.js`, buscá `configureContactLinks()` y completá los valores de:

- `instagram`
- `email`
- `web`

No se inventaron links ni teléfonos para que no aparezcan datos falsos.

## 7. Datos y privacidad

El sitio almacena para la participación: UID de Firebase, nombre visible de Google y, opcionalmente, URL de foto. No muestra el email en el sitio público.

Evitá publicar datos personales en comentarios o propuestas. Si el proyecto se usa de forma institucional, conviene acordar internamente una política de moderación y quién administra la cuenta.

## 8. Nota técnica

El proyecto usa módulos ESM del SDK web de Firebase 12.19.0 desde la CDN oficial. Si en el futuro querés pasar a un proyecto npm/Vite, podés migrarlo sin cambiar la estructura de datos.

## Fuentes oficiales de configuración

- Firebase Web setup: https://firebase.google.com/docs/web/setup
- Google Sign-In for Web: https://firebase.google.com/docs/auth/web/google-signin
- Firestore Security Rules: https://firebase.google.com/docs/firestore/security/overview
- GitHub Pages: https://docs.github.com/en/pages

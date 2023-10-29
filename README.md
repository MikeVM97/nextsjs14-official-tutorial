# BRIEF CONCEPTS

1. ## Getting Started

1.1. Start new Next.js project
`npx create-next-app@latest`

1.2. [Typescript plugin](https://nextjs.org/docs/app/building-your-application/configuring/typescript#typescript-plugin)

1.3. [Next.js official documentation](https://nextjs.org)

2. ## CSS Styling

2.1. Global styles (global.css)
2.2. CSS Framework (Tailwind)
2.3. CSS Modules (home.module.css)
2.4. **clsx** library (class names conditionally)
2.5. Other styling solutions (styled-jsx, styled-components, emotion, etc)

3. ## Optimizing Fonts and Images

3.1. **next/font** & **next/image** optimizes **Cumulative Layout Shift**
3.2. ¿ What is layout shift ?
**En el caso de las fuentes.**
Imagina que estás utilizando fuentes de Google en tu proyecto (Inter, por poner un ejemplo), en producción esta fuente no será cargada hasta que esté lista, es decir, se haya descargado e incrustado en el sitio web, mientras hace esto, la fuente que será cargada por defecto es una fuente alternativa o la del sistema (font-family: system-ui).
El hecho de cargar una fuente alternativa a la que definiste(solo mientras se descarga la fuente original), puede ocasionar un _CAMBIO DE DISEÑO_ inesperado, o traducido al inglés _LAYOUT SHIFT_, dado que esta fuente alternativa puede tener características(ejm: ancho, alto, peso, etc) muy diferentes que la fuente original(Inter) puede terminar ocasionando posibles desbordamientos, hacer que el tamaño, el espaciado o el diseño del texto cambien, desplazando elementos a su alrededor, es decir el sitio web sufrirá un CAMBIO DE DISEÑO que no era el esperado.
Entones, cada CAMBIO DE DISEÑO que tu aplicación web sufra por fuentes externas se irá acumulando, teniendo así un _CAMBIO DE DISEÑO ACUMULATIVO_, que traducido al inglés es _CUMULATIVE LAYOUT SHIFT_

_next/font_ existe por esta razón
Next.js optimiza automáticamente las fuentes en la aplicación cuando usa el módulo next/font. Lo hace descargando archivos de fuentes en el momento de la compilación y alojándolos con sus otros activos estáticos. Esto significa que cuando un usuario visita su aplicación, no hay solicitudes de fuentes de red adicionales que afectarían el rendimiento.

**En el caso de las imágenes**
El componente <Image />
Es una extensión de la etiqueta HTML <img> y viene con optimización automática de la imagen, como por ejemplo:

- Evitar el _CAMBIO DE DISEÑO_ automáticamente cuando se cargan las imágenes.
- Cambiar el tamaño de las imágenes para evitar enviar imágenes grandes a dispositivos con una ventana gráfica más pequeña.
- Carga diferida de imágenes de forma predeterminada (las imágenes se cargan a medida que ingresan a la ventana gráfica).
- Servir imágenes en formatos modernos, como WebP y AVIF.

Es una buena práctica establecer el ancho y el alto de las imágenes para evitar _CAMBIOS EN EL DISEÑO_; estas deben tener una relación de aspecto idéntica a la imagen de origen.
Ejemplo:

<Image
  src="/logo.svg"
  width={100}
  height={100}
  alt="Logo image"
/>

Aquí el componente Image tiene un ancho y alto establecido, con esto evitamos _CAMBIOS EN EL DISEÑO_.

4. ## Creating Layout and Pages

4.1. Pages
Next.js utiliza enrutamiento del sistema de archivos(file-system-routing) para servir cada página de tu aplicación.
Es decir que para agregar nuevas páginas a tu aplicación solo debes crear un directorio(el nombre del directorio será el nombre de la ruta) y dentro de éste crear un archivo que debe tener como nombre "page", la extensión del archivo dependerá de si usas javascript o typescript, y si usas React o no.
Ejemplo:

- app
  - page.tsx
  - login
    - page.tsx
  - register
    - page.tsx
  - home
    - page.tsx
    - dashboard
      - page.tsx

Cada page.tsx representa el contenido y la lógica de la página actual.
Si quisieras acceder a la ruta login lo harías de esta forma:
localhost:3000/login
Si quisieras acceder a la ruta dashboard lo harías de esta forma:
localhost:3000/home/dashboard

4.2. Layout
En Next.js, puede utilizar un archivo layout.tsx para crear una interfaz de usuario que se comparte entre varias páginas.
Una ventaja de utilizar el Layout es que durante la navegación, solo se actualizan los componentes de la página, mientras que el diseño no se vuelve a representar. En Next.js, esto se llama renderizado parcial([partial rendering](https://nextjs.org/docs/app/building-your-application/routing/linking-and-navigating#3-partial-rendering))
Ejemplo:

- app
  **- layout.tsx** «= RootLayout
  - page.tsx
  - login
    - page.tsx
  - register
    - page.tsx
  - home
    - layout.tsx
    - page.tsx
    - dashboard
      - page.tsx

El [root layout](https://nextjs.org/docs/app/building-your-application/routing/pages-and-layouts#root-layout-required) es requerido en la aplicación, los otros layouts no son obligatorios.
Cualquier interfaz de usuario que agregue al **root layout** se compartirá en todas las páginas de su aplicación. Puede utilizar el **root layout** para modificar sus etiquetas <html> y <body> y agregar metadatos.

5. ## Navigating between pages

5.1. The <Link /> Component

<Link /> le permite realizar navegación del lado del cliente con JavaScript. Aunque partes de su aplicación se procesan en el servidor, la navegación es más rápida y no hay una actualización de la página completa, lo que la hace sentir más como una aplicación web.
 
5.2. usePathName hook
[usePathName](https://nextjs.org/docs/app/api-reference/functions/use-pathname) es un hook de Next.js que te permite obtener la ruta actual en la que el usuario se encuentra en tu aplicación.

5.3. Automatic code-splitting and prefetching
**Code splitting**
Además de la navegación del lado del cliente, el código de Next.js divide automáticamente su aplicación por segmentos de ruta. Esto es diferente a una SPA tradicional, donde el navegador carga todo el código de su aplicación en la carga inicial.

Dividir el código por rutas significa que las páginas quedan aisladas. Si una determinada página arroja un error, el resto de la aplicación seguirá funcionando.

**Prefetching**
Además, en una producción, cada vez que aparecen componentes <Link> en la ventana gráfica del navegador, Next.js busca automáticamente el código para la ruta vinculada en segundo plano. Cuando el usuario haga clic en el enlace, el código de la página de destino ya estará cargado en segundo plano y la transición de la página será casi instantánea.

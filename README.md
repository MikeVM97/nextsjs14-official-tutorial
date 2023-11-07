# BRIEF CONCEPTS

1. ## Getting Started

   1.1. Start new Next.js project with `npx create-next-app@latest`

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

   Next.js optimiza automáticamente las fuentes en la aplicación cuando usa el módulo next/font. Lo hace descargando archivos de fuentes en el momento de la compilación y alojándolos con sus otros activos estáticos.

   Esto significa que cuando un usuario visita su aplicación, no hay solicitudes de fuentes de red adicionales que afectarían el rendimiento.

   **En el caso de las imágenes**

   El componente `<Image />`

   Es una extensión de la etiqueta HTML `<img>` y viene con optimización automática de la imagen, como por ejemplo:

   - Evitar el _CAMBIO DE DISEÑO_ automáticamente cuando se cargan las imágenes.
   - Cambiar el tamaño de las imágenes para evitar enviar imágenes grandes a dispositivos con una ventana gráfica más pequeña.
   - Carga diferida de imágenes de forma predeterminada (las imágenes se cargan a medida que ingresan a la ventana gráfica).
   - Servir imágenes en formatos modernos, como WebP y AVIF.

   Es una buena práctica establecer el ancho y el alto de las imágenes para evitar _CAMBIOS EN EL DISEÑO_; estas deben tener una relación de aspecto idéntica a la imagen de origen.

   Ejemplo:

   ```md
   <Image
     src="/logo.svg"
     width={100}
     height={100}
     alt="Logo image"
   />
   ```

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

   Una ventaja de utilizar el Layout es que durante la navegación, solo se actualizan los componentes de la página, mientras que el diseño no se vuelve a representar. En Next.js, esto se llama renderizado parcial([partial rendering](https://nextjs.org/docs/app/building-your-application/routing/linking-and-navigating#3-partial-rendering)).

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
   Cualquier interfaz de usuario que agregue al **root layout** se compartirá en todas las páginas de su aplicación. Puede utilizar el **root layout** para modificar sus etiquetas `<html>` y `<body>` y agregar metadatos.

5. ## Navigating between pages

   5.1. The `<Link />` Component

   `<Link />` le permite realizar navegación del lado del cliente con JavaScript. Aunque partes de su aplicación se procesan en el servidor, la navegación es más rápida y no hay una actualización de la página completa, lo que la hace sentir más como una aplicación web.

   5.2. usePathName hook

   [usePathName](https://nextjs.org/docs/app/api-reference/functions/use-pathname) es un hook de Next.js que te permite obtener la ruta actual en la que el usuario se encuentra en tu aplicación.

   5.3. Automatic code-splitting and prefetching

   **Code splitting**

   Además de la navegación del lado del cliente, el código de Next.js divide automáticamente su aplicación por segmentos de ruta. Esto es diferente a una SPA tradicional, donde el navegador carga todo el código de su aplicación en la carga inicial.

   Dividir el código por rutas significa que las páginas quedan aisladas. Si una determinada página arroja un error, el resto de la aplicación seguirá funcionando.

   **Prefetching**

   Además, en una producción, cada vez que aparecen componentes `<Link>` en la ventana gráfica del navegador, Next.js busca automáticamente el código para la ruta vinculada en segundo plano. Cuando el usuario haga clic en el enlace, el código de la página de destino ya estará cargado en segundo plano y la transición de la página será casi instantánea.

6. ## Databases

7. ## Fetching data

   ESCOGE COMO EXTRAER DATOS

   7.1. **API Layer**

   Las API son una capa intermediaria entre el código de su aplicación y la base de datos. Hay algunos casos en los que podría utilizar una API:

   - Si está utilizando servicios de terceros que proporcionan una API.
   - Si está obteniendo datos del cliente, querrá tener una capa API que se ejecute en el servidor para evitar exponer los secretos de su base de datos al cliente.

   En Next.js, puede crear endpoints de API utilizando controladores de ruta([Route Handlers](https://nextjs.org/docs/app/building-your-application/routing/route-handlers)).

   7.2. **Database queries**

   Cuando crea una aplicación full stack, también necesitará escribir lógica para interactuar con su base de datos. Para bases de datos relacionales como Postgres, puedes hacer esto con SQL o un ORM como [Prisma](https://www.prisma.io/).

   Hay algunos casos en los que es necesario escribir consultas a la base de datos:

   - Al crear sus endpoints de API, necesita escribir lógica para interactuar con su base de datos.
   - Si está utilizando React Server Components (obteniendo datos en el servidor), puede omitir la capa API y consultar su base de datos directamente sin correr el riesgo de exponer los secretos de su base de datos al cliente.

   Hay algunas otras formas de obtener datos con React y Next.js. No los cubriremos todos por cuestiones de tiempo. Si desea obtener más información, consulte la documentación sobre obtención de datos([Data Fetching](https://nextjs.org/docs/app/building-your-application/data-fetching/fetching-caching-and-revalidating)).

   7.3. **Utilizando Server Components para fetching de datos**

   Por defecto, las aplicaciones Next.js usan React Server Components(no directives) y usted puede optar por Client Components("use client" directive) cuando sea necesario. Existen algunos beneficios al recuperar datos con los React Server Components:

   - Los Server Components se ejecutan en el servidor, por lo que puede mantener costosas recuperaciones de datos y lógica en el servidor y solo enviar el resultado al cliente.
   - Los Server Components respaldan las promesas y brindan una solución más simple para tareas asincrónicas como la recuperación de datos. Puede utilizar la sintaxis `async/await` sin recurrir a las bibliotecas `useEffect`, `useState` o de recuperación de datos.
   - Dado que los Server Components se ejecutan en el servidor, puede consultar la base de datos directamente sin una capa API adicional.

   .

   7.4. **Utilizando SQL**

   Hay algunas razones por la cual usar SQL:

   - SQL es el estándar de la industria para consultar bases de datos relacionales (por ejemplo, los ORM generan SQL internamente).
   - Tener un conocimiento básico de SQL puede ayudarle a comprender los fundamentos de las bases de datos relacionales, permitiéndole aplicar sus conocimientos a otras herramientas.
   - SQL es versátil y le permite recuperar y manipular datos específicos.
   - El uso de [Vercel Postgress SDK](https://vercel.com/docs/storage/vercel-postgres/sdk) proporciona protección contra inyecciones de SQL.

   Puede llamar a `sql` dentro de cualquier Server Component, esta función le permite hacer consultas a su base de datos.

   `import { sql } from '@vercel/postgres'`

   7.5. **Que son las request waterfalls ?**

   Un "waterfall" se refiere a una secuencia de solicitudes de red que dependen de la finalización de solicitudes anteriores. En el caso de la recuperación de datos, cada solicitud solo puede comenzar una vez que la solicitud anterior haya devuelto datos.

   Por ejemplo:

   ```js
   const revenue = await fetchRevenue();
   const latestInvoices = await fetchLatestInvoices(); // wait for fetchRevenue() to finish
   const {
     numberOfInvoices,
     numberOfCustomers,
     totalPaidInvoices,
     totalPendingInvoices,
   } = await fetchCardData(); // wait for fetchLatestInvoices() to finish
   ```

   Este patrón no es necesariamente malo. Puede haber casos en los que desee cascadas porque desea que se cumpla una condición antes de realizar la siguiente solicitud. Por ejemplo, es posible que desee obtener primero el ID de un usuario y la información del perfil. Una vez que tengas la identificación, puedes proceder a buscar su lista de amigos. En este caso, cada solicitud depende de los datos devueltos por la solicitud anterior.

   Sin embargo, puede haber ocasiones en los que no desea este comportamiento ya que afectaría al rendimiento.

   7.6. **Parallel data fetching**

   Una forma común de evitar "waterfalls" es iniciar todas las solicitudes de datos al mismo tiempo, en paralelo.

   En JavaScript, puedes usar la función `Promise.all()` o la función `Promise.allSettled()` para iniciar todas las promesas al mismo tiempo.

   Por ejemplo:

   ```js
   const invoiceCountPromise = sql`SELECT COUNT(*) FROM invoices`;
   const customerCountPromise = sql`SELECT COUNT(*) FROM customers`;

   const data = await Promise.all([invoiceCountPromise, customerCountPromise]);
   ```

   Al usar este patrón, puedes:

   - Comenzar a ejecutar todas las recuperaciones de datos al mismo tiempo, lo que puede generar mejoras en el rendimiento.

   - Utilizar un patrón de JavaScript nativo que se pueda aplicar a cualquier biblioteca o marco.

8. ## Static and Dynamic Rendering

   8.1. **Renderizado estático**

   Next.js utiliza el renderizado estático por defecto.

   Con el renderizado estático, la obtención de datos (fetching data) y el renderizado se realiza en el servidor en tiempo de compilación(al momento de hacer deploy) o durante la revalidación([revalidation](https://nextjs.org/docs/app/building-your-application/data-fetching/fetching-caching-and-revalidating#revalidating-data)).

   Luego el resultado se puede distribuir y almacenar en caché en una red de entrega de contenido(CDN).

   8.2. **Renderizado dinámico**

   Con el renderizado dinámico, el contenido es renderizado en el servidor para cada usuario en **tiempo de solicitud**(cuando el usuario visita la página).

   Uno de los beneficios de implementar el renderizado dinámico es que te permite acceder a información que solo puede ser conocida en **tiempo de solicitud**, como las cookies o los parámetros de la URL.

   `import { unstable_noStore as noStore } from "next/cache"`

   Esta función le permite optar por no participar en el renderizado estático.

   Para ello, todo contenido que quiere que se renderize dinámicamente debe utilizar esta función, por ejemplo:

   ```js
   export async function fetchRevenue() {
     noStore();
     // ...El resto de la lógica
   }
   ```

9. ## Streaming

   9.1. **What is streaming ?**

   Streaming is a data transfer technique that allows you to break down a route into smaller "chunks" and progressively stream them from the server to the client as they become ready.

   By streaming, you can prevent slow data requests from blocking your whole page. This allows the user to see and interact with parts of the page without waiting for all the data to load before any UI can be shown to the user.

   Data fetching and rendering are initiated in parallel, so the user can see the UI as it becomes ready. This is different from the traditional waterfall approach, where data fetching and rendering are initiated sequentially, blocking the UI from rendering until all the data is ready.

   Streaming works well with React's component model, as each component can be considered a chunk.

   There are two ways you implement streaming in Next.js:

   - At the page level, with the _loading.tsx_ file.

   - For specific components, with `<Suspense />`.

     9.2. **Streaming a whole page with _loading.tsx_**

   El archivo [_loading.tsx_](https://nextjs.org/docs/app/building-your-application/routing/loading-ui-and-streaming) se encarga de mostrar contenido temporal hasta que la solicitud de datos se haya obtenido por completo, luego se reemplaza el contenido de _loading.tsx_ por el nuevo contenido.

   9.3. **Steaming a specific component with `<Suspense />`**

   Con el componente `<Suspense />` se logra lo mismo que con el archivo _loading_, pero para componentes específicos, por ejemplo:

   ```tsx
   import { Suspense } from "react";
   import { RevenueChartSkeleton } from "@/app/ui/skeletons";

   export default async function Page() {
     <Suspense fallback={<RevenueChartSkeleton />}>
       <RevenueChart />
     </Suspense>;
   }
   ```

10. ## Partial Prerendering

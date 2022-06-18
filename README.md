# Adopt-a-pet site

From the Frontend Masters course: Intermediate React v4 by Brian Holt.

Course Resources:
* [Frontend Masters website]()
* [Instructor's Notes]()
* [Github Repository](https://github.com/btholt/citr-v7-project)

## Tailwind

### Installation and Setup

`npm i -D tailwindcss@3.0.22 postcss@8.4.6 autoprefixer@10.4.2`
* `tailwindcss` is the package
* `postcss` is like babel for css
* `autoprefixer` takes care of prefixes like `--webkit-ui-property-name` **May no longer be necessary**

`npx tailwindcss init` = create a new tailwind installation
Add `mode: "jit"` to `tailwind.config.js` for just-in-time compliation.
Add configuration file `.postcssrc` to the root directory.

If you ever need to nuke Tailwind and start over:
`rm -rf .parcel-cache dist node_modules` then `npm i`

### Using Tailwind

Tailwind is not intended to be a design system (like Bootstrap). It's intended
to help you set up your applications.

1. Replace the css with these 3 lines:
```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```
2. Add config files
3. Add tailwind classes to each element. These are very long.

## Code splitting

Allow modules to be loaded dynamically.
`React.loadable` is no longer necessary.
`lazy` allows modules to be loaded lazily.
`Suspense` says "do this until you get everything back."

Once code is split, modules are only loaded once a page that needs them is.
This works out of the box in Parcel. It is more difficult to configure
properly in Webpack.

```js
// app.js: load Details and SearchParams lazily.
import { lazy, Suspense } from react;
const Details = lazy(() => import("./Details"));
const SearchParams = lazy(() => import("./SearchParams"));
// Wrap a Suspense block around the app:
// You wouldn't normally do this on the front page of your app.
<Suspense fallback={<h2>Loading...</h2>}></Suspense>
```

```js
// details.js: Make the modal load lazily.
import { lazy } from react;
const Modal = lazy(() => import("./Modal"));
```

 ## Server-side rendering (code splitting)

 Copy `App.js` to `ClientApp.js`.
 BrowserRouter can only happen on the client side, so move it there.
 Switch from using `render()` to `hydrate()` in `ClientApp`
 Change the index.html to use `ClientApp.js` instead of `App.js`
 Add `"targets"` block to `package.json`
 Add `server/index.js` to host Express server.

Site will now work even without JavaScript enabled (not that it would need to).
More importantly, once content is displayed, it is immediately usable.

## Streaming
Same as lazy loading?
`index.js`: `renderToString` --> `renderToNodeStream`
Note: that's actually depricated. What should we use instead?
1. Split the HTML into parts: `html.split("not rendered');`
2. `res.write(parts[0])` immediately: head and CSS
3. define `reactMarkup` (the router and app body)
4. `const stream = renderToNodeStream(reactMarkup)`
5. `stream.pipe(res, { end: false })` Let it know there is more coming.
6. `stream.on("end", () => res.end(parts[1]))` Close the connection. Optional.

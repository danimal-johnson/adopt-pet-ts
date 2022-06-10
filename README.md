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
* `autoprefixer` takes care of prefixes like `--webkit-ui-property-name`

`npx tailwindcss init` = create a new tailwind installation
Add `mode: "jit"` to `tailwind.config.js` for just-in-time compliation.
Add configuration file `.postcssrc` to the root directory.

If you ever need to nuke Tailwind and start over:
`rm -rf .parcel-cache dist node_modules` then `npm i`

### Using Tailwind

Tailwind is not intended to be a design system (like Bootstrap). It's intended
to help you set up your applications.
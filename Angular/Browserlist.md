# What is Browserslist?

Browserslist is a configuration tool that tells your build tools (for example Angular, PostCSS, Autoprefixer, and Babel) which browsers your project should support.

It’s not a browser itself; it defines target browsers for tasks such as:

- CSS autoprefixing (adding -webkit-, -moz- prefixes automatically)
- JavaScript transpiling (making modern JS compatible with older browsers)
- Polyfills (adding missing features for older browsers)

## How it works

You define browser versions in your project (usually in package.json or a .browserslistrc file). For example, in package.json:

```json
"browserslist": [
    "last 2 versions",
    "not dead",
    "> 0.2%"
]
```

What this means:What this means:

- last 2 versions → last 2 versions of all major browsers last 2 versions → last 2 versions of all major browsers
- not dead → ignores browsers without updates for > 24 months
- > 0.2% → supports browsers with more than 0.2% market sharenot dead → ignores browsers without updates for > 24 months

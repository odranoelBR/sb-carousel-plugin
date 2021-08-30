# Storyblok Integration Field Starter

> Custom integration field type plugin starter kit; Use this starter kit to make your external APIs available in Storyblok if multi-option and single-option are not enough.

## Project setup

```
npm install
```

### Compiles and hot-reloads for development

Startups local development server running Vue. You will only be able to develop this custom field type by enabling the [local development option](https://www.storyblok.com/docs/Guides/Creating-a-field-type-plugin#how-to-develop-plugins-locally) with Storyblok.

```
npm run serve
```

### Compiles and minifies for production

Exports the final plugin that can be included in Storyblok under `export.js`. Upload its content to your plugin. Make sure that your plugin name configured in `/src/Plugin.vue` is the equal to the name of your actual Storyblok plugin. Each plugin name is globally unqiue, which means that across Storyblok there will only be one plugin with that exact name for all users, even tho not all users will have access to your plugin.

```
npm run build
```

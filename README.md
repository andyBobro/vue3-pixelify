# vue3-pixelify

Vue 3 implementation of [react component by nikoferro](https://github.com/nikoferro/react-pixelify/) pixelizes image using html5 canvas

## Install:

```
npm install vue3-pixelify

# OR

yarn add vue3-pixelify
```

## Usage:

```
<script setup lang="ts">
import { Vue3Pixelify } from 'vue3-pixelify';
</script>


<template>
  <div>
    vue3-pixelify
    <Vue3Pixelify :src="'https://cdn2.thecatapi.com/images/egs.jpg'" />
  </div>
</template>
```

## Props:

| Property              | Type   | Default Value         | Description                                                                                                                                                                                                                                                                  | Required |
| --------------------- | ------ | --------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| src                   | String |                       | Source of the image                                                                                                                                                                                                                                                          | Yes      |
| pixelSize             | Int    | 4                     | Size of the pixel in the new pixelated image. if no value is set, the original image is displayed with no pixelated effect                                                                                                                                                   | No       |
| width                 | Int    | image original width  | You can use this prop to override the original width                                                                                                                                                                                                                         | No       |
| height                | Int    | image original height | You can use this prop to override the original height                                                                                                                                                                                                                        | No       |
| centered              | Bool   | false                 | If true, the pixels grid will be centered vertically and horizontally. Example: You choose a pixelSize of 10, but your image width or height cant be divided by an exact grid of 10x10 pixels. Setting this prop as **true** will set an offset that keeps the grid centered | No       |
| fillTransparencyColor | String | white                 | For images with transparency, you can set a value for the places where the image is transparent. Think of it as a background color for your pixelated image                                                                                                                  | No       |

## Recommended IDE Setup

- [VS Code](https://code.visualstudio.com/) + [Volar](https://marketplace.visualstudio.com/items?itemName=Vue.volar) (and disable Vetur) + [TypeScript Vue Plugin (Volar)](https://marketplace.visualstudio.com/items?itemName=Vue.vscode-typescript-vue-plugin).

## Type Support For `.vue` Imports in TS

TypeScript cannot handle type information for `.vue` imports by default, so we replace the `tsc` CLI with `vue-tsc` for type checking. In editors, we need [TypeScript Vue Plugin (Volar)](https://marketplace.visualstudio.com/items?itemName=Vue.vscode-typescript-vue-plugin) to make the TypeScript language service aware of `.vue` types.

If the standalone TypeScript plugin doesn't feel fast enough to you, Volar has also implemented a [Take Over Mode](https://github.com/johnsoncodehk/volar/discussions/471#discussioncomment-1361669) that is more performant. You can enable it by the following steps:

1. Disable the built-in TypeScript Extension
   1. Run `Extensions: Show Built-in Extensions` from VSCode's command palette
   2. Find `TypeScript and JavaScript Language Features`, right click and select `Disable (Workspace)`
2. Reload the VSCode window by running `Developer: Reload Window` from the command palette.


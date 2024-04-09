# README

## About

Svelte + Tailwind Template for Wails
Frontend updated to the latest packages and app versions for modern app development.

- @sveltejs/vite-plugin-svelte@2.4.5
- autoprefixer@10.4.15
- postcss@8.4.28
- svelte@4.2.0
- tailwindcss@3.3.3
- vite@4.4

## Live Development

To run in live development mode, run `wails dev` in the project directory. In another terminal, go into the `frontend`
directory and run `npm run dev`. The frontend dev server will run on http://localhost:34115. Connect to this in your
browser and connect to your application.

## Building

To build a redistributable, production mode package, use `wails build`.

# TODO
In no particular order

- [x] Detect All USB devices
- [x] Detect All VIA devices
- [x] Select a device
- [x] Get All VIA definitions for offline support
- [x] Check for support status (official/sideload/missing) PARTIAL COMPLETE
- [ ] Read in keyboard JSON files to GO structs
- [ ] Frontend Keyboard Rendering
- [ ] Add sideloading support
- [ ] Write design tab converter from "raw" via JSON format (github) to compiled ones
- [ ] Add VIA versions specific versions 9 - 12 keycodes etc
- [ ] Read in Keymap, Options, Macros, Lighting from HID device
- [ ] Keycode support
- [ ] Writing macros
- [ ] Keyboard Tester Support (probably optional at this point)
- [ ] VIA Custom Menu support
- [ ] Frontend Menus support
- [ ] App Settings and saving
- [ ] Language Localisation
- [ ] Analog Support (future)
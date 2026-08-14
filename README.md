# WEH Worldwide — Vue 3 / Vite recreation

A Vue 3 implementation of the reference site's page flow:

- full-screen tap-to-enter intro
- fixed minimal header
- oversized editorial hero typography
- 01 GXDS
- 02 MEMBERS
- 03 JOIN
- 04 COLLABS
- footer
- fixed custom audio player
- desktop + mobile responsive layout

The code is componentized, while the content and replaceable media live in one config file.

## 1. Requirements

Install Node.js first. This project is configured for modern Vue 3 + Vite.

Check your installation:

```bash
node -v
npm -v
```

## 2. Run locally

Unzip the project, then open a terminal in the project folder:

```bash
cd weh-vue-clone
npm install
npm run dev
```

Vite will print a local URL, normally similar to:

```text
http://localhost:5173/
```

Open that address in your browser.

## 3. Production build

```bash
npm run build
```

The production files are generated in:

```text
dist/
```

To test the production build locally:

```bash
npm run preview
```

## 4. Replace the image, cover and music

Open:

```text
src/siteConfig.js
```

You only need to edit this object:

```js
export const media = {
  introGraphic: 'YOUR_IMAGE_OR_GIF_URL',
  albumCover: 'YOUR_COVER_URL',
  audioUrl: 'YOUR_MUSIC_URL',
}
```

### Recommended: use local files before final deployment

Put your files here:

```text
public/media/intro.gif
public/media/cover.jpg
public/media/song.mp3
```

Then change the config to:

```js
export const media = {
  introGraphic: '/media/intro.gif',
  albumCover: '/media/cover.jpg',
  audioUrl: '/media/song.mp3',
}
```

This is more reliable than hotlinking another website.

## 5. Change names / Discord links

All of the data is also in:

```text
src/siteConfig.js
```

Edit these exports:

- `gxds`
- `members`
- `links.join`
- `links.collabs`
- `track`

The Vue template updates automatically.

## 6. Change spacing without breaking the layout

The important layout values are at the top of:

```text
src/styles.css
```

```css
:root {
  --page-x: clamp(16px, 2.15vw, 38px);
  --header-h: 62px;
  --section-top: clamp(96px, 10.5vw, 170px);
  --section-bottom: clamp(120px, 12vw, 195px);
}
```

If you want to fine-tune the page after comparing it side-by-side with your target browser size, start with those four variables.

The main two-column relationship is controlled here:

```css
.directory-layout,
.feature-section__body {
  grid-template-columns: minmax(235px, 29.2vw) minmax(0, 1fr);
}
```

## 7. Intro behavior

The intro is in:

```text
src/components/IntroOverlay.vue
```

Clicking/tapping it:

1. removes the page scroll lock
2. transitions the intro upward
3. reveals the page
4. asks the audio player to start playback

Browsers can still block remote audio in some situations, so the play button remains usable.

## 8. Music player

The player is in:

```text
src/components/MusicPlayer.vue
```

It supports:

- play / pause
- current time
- duration
- seeking
- mute / unmute
- mobile responsive layout

## 9. Deploy to Netlify

Push the project to GitHub or upload it to Netlify.

Use:

```text
Build command: npm run build
Publish directory: dist
```

No backend is required.

## 10. Project structure

```text
weh-vue-clone/
├── public/
│   └── media/
├── src/
│   ├── components/
│   │   ├── HeroSection.vue
│   │   ├── IntroOverlay.vue
│   │   ├── MusicPlayer.vue
│   │   ├── SectionLabel.vue
│   │   └── SiteHeader.vue
│   ├── App.vue
│   ├── main.js
│   ├── siteConfig.js
│   └── styles.css
├── .gitignore
├── index.html
├── package.json
├── vite.config.js
└── README.md
```

## Temporary remote media

The starter config intentionally uses remote URLs so the layout has visible content immediately. Replace those URLs with assets you have permission to use before treating this as a final production site.

## Member images / hover preview

The GXDS and MEMBERS lists now include the same kind of desktop image-reveal interaction: hover a name and a large portrait appears in the left column without changing the list geometry.

Temporary member photos are configured in `src/siteConfig.js` with deterministic `picsum.photos` URLs. Replace any member image like this:

```js
{
  name: 'Weh Sophie',
  image: '/media/sophie.jpg',
}
```

Put the replacement file at `public/media/sophie.jpg`. No Vue template changes are required.

## Hybrid roster version

This build intentionally keeps the original intro/loading screen, white hero, header, JOIN, COLLABS, footer, and music player. Only the GXDS + MEMBERS area uses the dark image-card roster treatment.

Replace the roster background in `src/siteConfig.js`:

```js
rosterBackground: '/media/your-background.jpg'
```

Member images are also in `src/siteConfig.js`. Put your own files in `public/media/` and replace each `image` URL with `/media/your-file.jpg`.

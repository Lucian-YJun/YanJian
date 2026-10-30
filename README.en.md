# 砚笺 YanJian · a two-mode SiYuan theme

> **One study, two hours of the day — paper in daylight, inkstone by night.**
> A restrained, CJK-first theme: indigo for interaction, cinnabar for warning,
> and a paper that stays out of the way of the text.

- **笺 / light**: paper `#FAF7F1` · indigo `#2F5D8A` · cinnabar `#B3362A`
- **砚 / dark**: warm ink `#26241F` · lamp-lit indigo `#82A7CC` · cinnabar `#E07B6D`

The two palettes are designed independently — not an inversion.

## Requirements

- SiYuan **3.8.0 or later** (declared via `minAppVersion`).
- Desktop and mobile frontends are both supported (`frontends: ["all"]`).

## Install

### Manual

1. `Settings → Appearance → Themes → open theme folder`
   (or `{workspace}/conf/appearance/themes/`).
2. Create a folder named **`YanJian`** (must match `theme.json`'s `name`).
3. Copy `theme.json`, `theme.css`, `theme-dark.css`, `README.md` (plus
   `README.en.md`, `LICENSE`, `icon.png`, `preview.png` if you like) into it.
4. Refresh the theme list and pick 砚笺 in both light and dark modes.

### Marketplace

After the theme is accepted: `Settings → Marketplace → Themes → search "砚笺 / YanJian"`.

## How the two modes load

The client always loads `theme.css`; `theme-dark.css` is **not** linked
separately. The dark palette is pulled in by the first line of `theme.css`
(`@import url("theme-dark.css")`) and scoped to `:root[data-theme-mode="dark"]`.
Because the dark rules carry that prefix, they override the light ones.

## Customising

All interactive colours derive from one token:

```css
:root {
  --b3-theme-primary: #345F73;
  --b3-theme-primary-light: rgba(52, 95, 115, .54);
  --b3-theme-primary-lighter: rgba(52, 95, 115, .38);
  --b3-theme-primary-lightest: rgba(52, 95, 115, .12);
}
```

The four-value scale must be updated together (the client treats them as
independent variables); hover/active states, focus halo, button shadow, switch
hover, callout tint and dark selection now derive from `--b3-theme-primary`
via `color-mix()`. **Semantic** colours (inline links, code, block references,
callouts' functional colours) are deliberately independent of the primary.

Editor measure: `--yj-measure` (`calc(16px × 46)`, 44 / 36 at narrower
breakpoints). Spacing, easing and duration tokens are listed in the Chinese
README (section 三).

## Preview pages

`preview/` holds 12 static pages (light/dark × main/states/mobile/focus/menu/hover)
built from real SiYuan class names. They are development fixtures and are not
part of the installed theme.

## License

MIT — see `LICENSE`. The theme is pure CSS (no `theme.js`, no network access,
no configuration writes).

# Portfolio — Chidera Paul Ogbu

A high-performance, interactive developer portfolio built with **SvelteKit**, **Three.js**, and **Threlte** — featuring a real-time animated 3D character, cinematic lighting, and a full project showcase.

---

## Table of Contents

- [Tech Stack](#tech-stack)
- [Setup Instructions](#setup-instructions)
- [Project Architecture](#project-architecture)
- [3D System Architecture](#3d-system-architecture)
- [Animation Decisions](#animation-decisions)
- [Performance Optimization](#performance-optimization)
- [Accessibility Considerations](#accessibility-considerations)
- [Trade-offs Made](#trade-offs-made)

---

## Tech Stack

| Layer | Technology |
|---|---|
| Framework | [SvelteKit](https://kit.svelte.dev) (Svelte 5 runes) |
| 3D Engine | [Three.js](https://threejs.org) via [Threlte](https://threlte.xyz) |
| Animation | GSAP (UI), Three.js AnimationMixer (3D), Threlte `<Float>` |
| Styling | Vanilla CSS (CSS custom properties, no utility framework) |
| Language | TypeScript |
| Build tool | Vite |
| 3D Model | Mixamo GLB (GLTF 2.0 format) |

---

## Setup Instructions

### Prerequisites

- [Node.js](https://nodejs.org) v18 or higher
- npm v9 or higher

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/Xavviieerr/portfolio.git
cd portfolio

# 2. Install dependencies
npm install

# 3. Start the dev server
npm run dev
```

The site will be available at `http://localhost:5173` (or the next available port).

### Build for Production

```bash
npm run build
npm run preview  # preview the production build locally
```

---

## Project Architecture

```
src/
├── lib/
│   ├── three/
│   │   ├── Scene.svelte        # WebGL canvas, camera, lights, shadows, environment
│   │   └── Character.svelte    # GLTF model loader, animation, idle sway
│   ├── components/
│   │   ├── ProjectWorldSection.svelte  # Interactive project showcase
│   │   ├── personality.svelte          # About / personality section
│   │   └── Footer.svelte
│   ├── sections/
│   │   └── Home.svelte         # Hero section — integrates the 3D scene
│   └── index.ts                # Public lib exports
├── routes/
│   └── +page.svelte            # Root page, composes all sections
public/
└── models/
    └── Housedancing.glb        # 3D character model (not committed)
```

### Data Flow

```
+page.svelte
  └── Home.svelte
        ├── Scene.svelte          ← creates WebGL context
        │     ├── Camera
        │     ├── Lights (×5)
        │     ├── Float
        │     │     └── Character.svelte  ← loads GLB, plays animation
        │     ├── ContactShadows
        │     └── Environment (IBL)
        └── [text content + GSAP entrance animations]
```

---

## 3D System Architecture

### Why Threlte?

Threlte is a Svelte-native wrapper around Three.js. It was chosen because:

- **Declarative scene graph** — Lights, cameras, and meshes are Svelte components. The scene reads like HTML, making it maintainable.
- **Svelte store reactivity** — `useGltf()` returns a Svelte store; the template reactively shows the model when it resolves, with no manual event listeners.
- **Lifecycle integration** — `useTask()` hooks into Threlte's unified render loop, eliminating the need to manage `requestAnimationFrame` manually.
- **No extra bundle cost** — Threlte is a thin wrapper; Three.js itself is the only heavy dependency.

### Scene Structure

| Component | Responsibility |
|---|---|
| `<Canvas shadows>` | Creates `WebGLRenderer`, enables `PCFSoftShadowMap` shadow maps |
| `<T.PerspectiveCamera makeDefault>` | Registers the active render camera (fov 45°, z=4.2) |
| `<T.AmbientLight>` | Low-intensity fill to prevent pure-black voids |
| `<T.DirectionalLight castShadow>` | Key light — warm, top-right, drives shadow maps |
| `<T.DirectionalLight>` ×2 | Rim (teal, behind) + fill (dim, front-low) |
| `<T.PointLight>` | Foot-level stage pool light |
| `<Float>` | Sinusoidal bobbing wrapper around the character |
| `<Character />` | GLTF loader + AnimationMixer + idle sway |
| `<ContactShadows>` | Soft blob shadow under the character's feet |

---

## Animation Decisions

### 1. Dance Animation (Three.js AnimationMixer)

The character's dance is driven by a skeletal animation embedded in the GLB file — specifically a Mixamo `HouseWalking` or similar clip. The implementation auto-plays the **first available clip** regardless of name, making it portable across any Mixamo export:

```ts
const first = Object.values($actions)[0];
first.reset().fadeIn(0.4).play();
```

- **`fadeIn(0.4)`** — Crossfades from the T-pose into the dance over 400ms, preventing a jarring snap on load.
- **Loop mode defaults to `THREE.LoopRepeat`** — the animation runs indefinitely.

### 2. Idle Sway (useTask)

A subtle Y-axis rotation is applied every frame using a sine wave:

```ts
meshRef.rotation.y = Math.sin(t * 0.5) * 0.08;
```

- **Period:** ~12.5 seconds per full left-right cycle (slow, contemplative)
- **Amplitude:** ±0.08 radians (≈ ±4.6°) — barely perceptible, adds biological life
- **Rationale:** The dance animation occupies the full body, but a small head-turn effect makes the character feel aware of being watched

### 3. Float (Threlte Float component)

```svelte
<Float speed={0.9} rotationIntensity={0.08} floatIntensity={0.25}>
```

Adds an organic vertical bob and micro-rotation. This runs independently of the dance animation and compounds with it, making the character feel grounded in air (like a hologram).

### 4. GSAP Entrance (UI layer)

The scene column animates in on mount:

```ts
gsap.set(sceneRef, { opacity: 0, x: 40 });
tl.to(sceneRef, { x: 0, opacity: 1, duration: 1.1, ease: 'power2.out' }, 0.2);
```

Slides in from the right with a 200ms delay, letting the text content begin its entrance first, then the 3D character sweeps in.

---

## Performance Optimization

### 1. Single GLB Load

`useGltf('/models/Housedancing.glb')` uses Threlte's internal caching via Three.js's `LoadingManager`. If the same URL is requested twice (e.g. the desktop scene and mobile background scene both mount), the GLB is parsed once and the scene is cloned.

### 2. Shadow Map Budget

Shadow maps are expensive. The scene restricts them to a single light (the key `DirectionalLight`). The shadow camera frustum is kept tight (`left/right ±4`, `top 6`) to concentrate the 2048×2048 shadow map texel budget on the character, not empty space.

### 3. ContactShadows vs Shadow Maps

The soft ground shadow uses `<ContactShadows>` (a secondary render pass) rather than enabling `receiveShadow` on a floor plane. This avoids a dedicated floor mesh and the shadow map stretching artefacts that come with it, at the cost of one extra render pass.

### 4. Float as CSS-like Animation

`<Float>` computes its transforms in a single shared `useTask` on the Threlte render loop rather than spawning a new `requestAnimationFrame`. All per-frame work happens in one unified loop tick.

### 5. Mobile Layout Separation

On mobile (`≤768px`), the desktop `.scene-col` is `display:none` and a separate full-bleed `.scene-mobile-bg` instance renders. This avoids rendering a half-width 3D scene on a small viewport — each version is sized for its context.

### 6. Environment IBL Disabled by Default

The `<Environment>` component (HDR image-based lighting) is commented out:

```svelte
<!-- <Environment backgroundIntensity={0.15} /> -->
```

IBL requires an HDR texture download and an additional render pass for reflections. The custom 5-light rig achieves a comparable look at zero texture cost.

---

## Accessibility Considerations

### Reduced Motion

Three.js and Threlte do not natively observe `prefers-reduced-motion`. As a trade-off, the 3D animation runs regardless of the user's OS motion preference. **Recommended future improvement:**

```ts
const prefersReducedMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches;

if (!prefersReducedMotion) {
  first.reset().fadeIn(0.4).play();
}
```

This would pause the dance animation (leaving the character in a static T-pose) for users who have opted out of motion.

### Canvas Accessibility

A `<canvas>` element has no implicit ARIA role. **Recommended future improvement:** add a descriptive label to the canvas:

```html
<canvas role="img" aria-label="Animated 3D character dancing — decorative" />
```

Threlte does not expose a direct prop for this; it would require accessing the canvas DOM node via `bind:this` on the `<Canvas>` component.

### Keyboard Navigation

The hero section contains only a single interactive element — the scroll button — which is a native `<button>` element and is fully keyboard accessible (tab-focusable, Enter/Space activates it).

### Colour Contrast

Text content in the hero section targets a minimum 4.5:1 contrast ratio against the dark background (`#0a0e0c`). The heading (`#dce1de` on `#0a0e0c`) achieves approximately 16:1.

### Screen Reader Content

The 3D scene is purely decorative. Hiding it from assistive technology would be ideal:

```html
<div class="scene-col" aria-hidden="true">
```

This prevents screen readers from announcing the canvas as an unknown interactive element.

---

## Trade-offs Made

| Decision | Trade-off | Rationale |
|---|---|---|
| **Threlte over raw Three.js** | Adds a dependency; abstracts some Three.js internals | Declarative scene graph is far more maintainable in a Svelte project. The wrapper is thin enough that ejecting to raw Three.js is always possible |
| **Mixamo GLB (cm scale)** | Requires `scale={[0.015, 0.015, 0.015]}` correction | Mixamo is the easiest source of high-quality rigged, animated humanoids. The scale correction is a one-time, well-documented fix |
| **5-light rig vs IBL** | More manual setup; IBL would be 2–3 lines | The custom rig gives precise artistic control over the teal/warm contrast palette. IBL preset maps rarely match a custom colour scheme |
| **ContactShadows vs floor plane** | Extra render pass; shadow doesn't react to scene geometry | Produces a far softer, more cinematic shadow without adding a visible floor mesh |
| **No `prefers-reduced-motion` guard** | Runs animation for all users | Acceptable for a portfolio; the animation is not essential to understanding content. Flagged for future improvement |
| **Two `<Scene>` instances (desktop + mobile)** | Two WebGL contexts, two `AnimationMixer` instances | Simpler than a single shared canvas that repositions on breakpoint. WebGL context limit on most browsers is 16; two is negligible |
| **GLB not committed to repo** | Requires manual download step | Mixamo models are 4–15 MB. Committing binary assets to Git degrades repository performance and clone times |
| **Environment IBL disabled** | Loses realistic reflections on character surfaces | Removes one texture download and one render pass. The 5-light rig compensates visually |

---

## License

MIT — see [LICENSE](./LICENSE) for details.

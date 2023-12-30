+++
title = "Extra content"
+++

This is a place for extra content to be temporarily placed until they find a good location.

---

## Quick notes

For the elara handbook images - use this template:

```md
{image} ../images/raster/fbd-gravity.png
:alt: Free body diagram single force
:width: 400px
:align: center
```

Elara UI/UX guidelines: high accessibility, visual comfort, and clarity are the main priorities.

Project Elara research papers should use literate programming to ensure reproducible research, like org-mode. They should be written in a specialized markdown syntax with support for executable code blocks (like MyST markdown), which is then compiled to LaTeX and HTML (with the Elara academic theme). Design the Elara academic theme in Figma.

Maybe eventually add a command palette/fileswitcher to Elara Hub, and add a Obsidian-style graph view like [this](https://github.com/blinpete/wiki-graph).

Implement responsivity to `elara-ui` by setting breakpoint functions in Component trait.

For Elara UI also create pure CPU-based backend that uses a Rust-ported version of `fenster`. Users can choose which backend they want: 

- GPU backend is faster and leads to smoother UI rendering but uses a lot of battery and can be glitchy if graphics drivers aren't working correctly, and may not be compatible with very old devices
- For low-powered devices CPU rendering is better, but it is much slower

For Elara array first optimize the CPU version as much as possible. E.g. the arithmetic operations should return a view rather than a completely new array, or use clone-on-write. The CPU backend should always be available, even if the GPU backend is faster.

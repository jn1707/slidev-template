---
layout: cover
class: text-left
transition: fade-out
dragPos:
  KU: 13,13,159,57
  EU: 224,13,203,52
---

<div class="mt-16">

# Your Presentation Title

Subtitle or Brief Description

</div>

<div class="mt-16">

**Date:** DD Month YYYY

**Institution:** Your Institution

**By:** Your Name

</div>

<!-- Add your logos here - uncomment and adjust paths as needed -->
<!-- <img v-drag="'KU'" src="./logos/ku_logo.png"/> -->
<!-- <img v-drag="'EU'" src="./logos/EU.png"/> -->

<style>
h1 {
  font-weight: 900;
  color: #8B0000;
}
</style>

<template v-slot:footer>
  <div style="position: absolute; bottom: 1.5em; right: 2em; font-size: 1.1em; color: #8B0000; opacity: 0.7;">
    Slide {{$slidev.nav.current}} / {{$slidev.nav.total}}
  </div>
</template>

---
layout: default
transition: fade-out
---

# Introduction

<div class="mt-4">

Brief overview of your topic

- Key point 1
- Key point 2
- Key point 3

</div>

<div style="position: absolute; bottom: 1em; right: 1.5em; font-size: 1em; color: #8B0000; opacity: 0.7;">
    <SlideCurrentNo />
</div>

<style>
h1 {
  font-weight: 900;
  color: #8B0000;
}
</style>

---
layout: default
transition: fade-out
---

# Regular Slide with Image

<div class="mt-4">

Add your content here with supporting image

</div>

<div style="display: flex; justify-content: center; align-items: center; margin-top: 2rem;">
  <img 
    src="./images/your-image.png"
    alt="Description of your image"
    style="max-width: 90%; max-height: 400px; border-radius: 12px; box-shadow: 0 4px 24px rgba(139,0,0,0.10); object-fit: contain;"
  />
</div>

<div style="position: absolute; bottom: 1em; right: 1.5em; font-size: 1em; color: #8B0000; opacity: 0.7;">
    <SlideCurrentNo />
</div>

<style>
h1 {
  font-weight: 900;
  color: #8B0000;
}
</style>

---
layout: default
transition: fade-out
---

# Two Column Layout

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 2rem; margin-top: 2rem;">
  <div>
    <h3>Left Column</h3>
    <ul>
      <li>Point 1</li>
      <li>Point 2</li>
      <li>Point 3</li>
    </ul>
  </div>
  <div>
    <h3>Right Column</h3>
    <ul>
      <li>Point A</li>
      <li>Point B</li>
      <li>Point C</li>
    </ul>
  </div>
</div>

<div style="position: absolute; bottom: 1em; right: 1.5em; font-size: 1em; color: #8B0000; opacity: 0.7;">
    <SlideCurrentNo />
</div>

<style>
h1 {
  font-weight: 900;
  color: #8B0000;
}
</style>

---
layout: default
transition: fade-out
---

# Math Example

<div class="mt-4">

Slidev supports LaTeX math rendering with KaTeX:

Inline math: $E = mc^2$

Block math:
$$
\int_{-\infty}^{\infty} e^{-x^2} dx = \sqrt{\pi}
$$

</div>

<div style="position: absolute; bottom: 1em; right: 1.5em; font-size: 1em; color: #8B0000; opacity: 0.7;">
    <SlideCurrentNo />
</div>

<style>
h1 {
  font-weight: 900;
  color: #8B0000;
}
</style>

---
class: py-10
transition: fade-out
---

# Summary

<div class="mt-8">

## Key Takeaways

<div class="mt-6" style="display: flex; flex-direction: column; gap: 1.5rem;">

<div v-click style="background: linear-gradient(90deg, #fff5f5 0%, #f8f9fa 100%); border: 2px solid #8B0000; border-radius: 12px; box-shadow: 0 4px 16px rgba(139,0,0,0.08); padding: 1.5rem;">
  <span style="font-size: 1.1em; font-weight: bold; color: #8B0000;">Summary Point 1</span>
  <div class="mt-2">Brief explanation of the first main finding or conclusion</div>
</div>

<div v-click style="background: linear-gradient(90deg, #fff5f5 0%, #f8f9fa 100%); border: 2px solid #8B0000; border-radius: 12px; box-shadow: 0 4px 16px rgba(139,0,0,0.08); padding: 1.5rem;">
  <span style="font-size: 1.1em; font-weight: bold; color: #8B0000;">Summary Point 2</span>
  <div class="mt-2">Brief explanation of the second main finding or conclusion</div>
</div>

<div v-click style="background: linear-gradient(90deg, #fff5f5 0%, #f8f9fa 100%); border: 2px solid #8B0000; border-radius: 12px; box-shadow: 0 4px 16px rgba(139,0,0,0.08); padding: 1.5rem;">
  <span style="font-size: 1.1em; font-weight: bold; color: #8B0000;">Summary Point 3</span>
  <div class="mt-2">Brief explanation of the third main finding or conclusion</div>
</div>

</div>

</div>

<div style="position: absolute; bottom: 1em; right: 1.5em; font-size: 1em; color: #8B0000; opacity: 0.7;">
    <SlideCurrentNo />
</div>

<style>
h1 {
  font-weight: 900;
  color: #8B0000;
}
</style>

---
layout: center
class: text-center
transition: fade-out
---

# Thank You

**Your Presentation Title**

<div class="mt-5">

**Your Name**  
*Your Institution*  
*Date*

</div>

<div v-click class="mt-5">

### References

[Paper Title 1](https://arxiv.org/abs/1234.5678) - First Author et al., Year

[Paper Title 2](https://arxiv.org/abs/9876.5432) - Second Author et al., Year

[Additional Resource](https://example.com) - Description

</div>

<div class="mt-8 text-lg opacity-70">

Questions?

</div>

<style>
h1 {
  font-weight: 900;
  color: #8B0000;
}
</style>

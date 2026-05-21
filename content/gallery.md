---
title: "Photo Gallery"
description: "A Display of Community Submitted Photos of Heroes of Lexington"
featured_image: "/images/citizenheroes.png"
---

<div class="gallery-page">
  <p>Click any photo to spotlight it, then use the arrows or the close button to navigate. These photos are a selected group of photos from all submissions.</p>
  <div class="gallery-grid" id="galleryGrid"></div>

  <div class="spotlight-overlay" id="spotlightOverlay" aria-hidden="true">
    <div class="spotlight-backdrop" onclick="closeSpotlight()"></div>
    <div class="spotlight-panel" role="dialog" aria-modal="true" aria-labelledby="spotlightCaption">
      <button class="spotlight-close" onclick="closeSpotlight()" aria-label="Close spotlight">×</button>
      <button class="spotlight-nav spotlight-prev" onclick="prevPhoto()" aria-label="Previous photo">‹</button>
      <div class="spotlight-content">
        <img id="spotlightImage" src="" alt="Spotlight photo" />
        <div id="spotlightCaption" class="spotlight-caption"></div>
      </div>
      <button class="spotlight-nav spotlight-next" onclick="nextPhoto()" aria-label="Next photo">›</button>
    </div>
  </div>
</div>

<style>
.gallery-page {
  max-width: 1200px;
  margin: 0 auto;
  padding: 1rem 0;
}
.gallery-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: 1rem;
}
.gallery-item {
  overflow: hidden;
  border-radius: 12px;
  box-shadow: 0 10px 24px rgba(0,0,0,.12);
}
.gallery-item img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
  cursor: pointer;
  transition: transform .25s ease, filter .25s ease;
}
.gallery-item img:hover {
  transform: scale(1.05);
  filter: brightness(1.02);
}
.spotlight-overlay {
  position: fixed;
  inset: 0;
  display: none;
  justify-content: center;
  align-items: center;
  z-index: 9999;
}
.spotlight-overlay.active {
  display: flex;
}
.spotlight-backdrop {
  position: absolute;
  inset: 0;
  background: rgba(0,0,0,.75);
}
.spotlight-panel {
  position: relative;
  max-width: 92vw;
  max-height: 92vh;
  width: min(1000px, 90vw);
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 1rem;
  padding: 1rem;
  z-index: 1;
}
.spotlight-content {
  flex: 1 1 auto;
  min-width: 0;
  text-align: center;
}
.spotlight-content img {
  max-width: 100%;
  max-height: 75vh;
  border-radius: 12px;
  box-shadow: 0 18px 50px rgba(0,0,0,.35);
}
.spotlight-caption {
  margin-top: 1rem;
  color: white;
  font-size: clamp(1rem, 1.1vw, 1.2rem);
  line-height: 1.4;
}
.spotlight-close,
.spotlight-nav {
  background: rgba(0,0,0,.6);
  color: white;
  border: none;
  border-radius: 999px;
  width: 44px;
  height: 44px;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: background .2s ease;
}
.spotlight-close:hover,
.spotlight-nav:hover {
  background: rgba(0,0,0,.85);
}
.spotlight-prev,
.spotlight-next {
  font-size: 1.75rem;
  min-width: 54px;
  min-height: 54px;
}
.spotlight-close {
  position: absolute;
  top: -18px;
  right: -18px;
  width: 48px;
  height: 48px;
  font-size: 1.5rem;
}
@media (max-width: 720px) {
  .spotlight-panel {
    flex-direction: column;
  }
  .spotlight-prev,
  .spotlight-next {
    width: 42px;
    height: 42px;
  }
}
</style>

<script>
const photos = [
  { src: "/images/gallery/aapi1.jpg", caption: "Photo placeholder for aapi1" },
  { src: "/images/gallery/aapi2.jpg", caption: "Photo placeholder for aapi2" },
  { src: "/images/gallery/aapi3.jpg", caption: "Photo placeholder for aapi3" },
  { src: "/images/gallery/allstars.jpeg", caption: "Photo placeholder for allstars" },
  { src: "/images/gallery/bridge.jpg", caption: "Photo placeholder for bridge" },
  { src: "/images/gallery/bruce.jpeg", caption: "Photo placeholder for bruce" },
  { src: "/images/gallery/calex1.jpg", caption: "Photo placeholder for calex1" },
  { src: "/images/gallery/calex2.jpg", caption: "Photo placeholder for calex2" },
  { src: "/images/gallery/counselor.jpeg", caption: "Photo placeholder for counselor" },
  { src: "/images/gallery/display.png", caption: "Photo placeholder for display" },
  { src: "/images/gallery/farmers.jpeg", caption: "Photo placeholder for farmers" },
  { src: "/images/gallery/festival1.jpg", caption: "Photo placeholder for festival1" },
  { src: "/images/gallery/festival2.jpg", caption: "Photo placeholder for festival2" },
  { src: "/images/gallery/festival3.jpg", caption: "Photo placeholder for festival3" },
  { src: "/images/gallery/festival4.jpg", caption: "Photo placeholder for festival4" },
  { src: "/images/gallery/festival5.jpg", caption: "Photo placeholder for festival5" },
  { src: "/images/gallery/festival6.jpg", caption: "Photo placeholder for festival6" },
  { src: "/images/gallery/festival7.jpg", caption: "Photo placeholder for festival7" },
  { src: "/images/gallery/festival8.jpg", caption: "Photo placeholder for festival8" },
  { src: "/images/gallery/festival9.jpg", caption: "Photo placeholder for festival9" },
  { src: "/images/gallery/greenteam.png", caption: "Photo placeholder for greenteam" },
  { src: "/images/gallery/helpfulhands.jpg", caption: "Photo placeholder for helpfulhands" },
  { src: "/images/gallery/kolex.jpg", caption: "Photo placeholder for kolex" },
  { src: "/images/gallery/lexlux.jpg", caption: "Photo placeholder for lexlux" },
  { src: "/images/gallery/musicfestival.jpg", caption: "Photo placeholder for musicfestival" },
  { src: "/images/gallery/musicians.jpeg", caption: "Photo placeholder for musicians" },
  { src: "/images/gallery/parade1.jpeg", caption: "Photo placeholder for parade1" },
  { src: "/images/gallery/parade2.jpg", caption: "Photo placeholder for parade2" },
  { src: "/images/gallery/parade3.jpeg", caption: "Photo placeholder for parade3" },
  { src: "/images/gallery/party.jpg", caption: "Photo placeholder for party" },
  { src: "/images/gallery/pta.png", caption: "Photo placeholder for pta" },
  { src: "/images/gallery/robot.jpg", caption: "Photo placeholder for robot" },
  { src: "/images/gallery/scouts1.jpg", caption: "Photo placeholder for scouts1" },
  { src: "/images/gallery/scouts2.jpg", caption: "Photo placeholder for scouts2" },
  { src: "/images/gallery/scouts3.jpg", caption: "Photo placeholder for scouts3" },
  { src: "/images/gallery/volunteers.jpg", caption: "Photo placeholder for volunteers" }
];

const galleryGrid = document.getElementById('galleryGrid');
const spotlightOverlay = document.getElementById('spotlightOverlay');
const spotlightImage = document.getElementById('spotlightImage');
const spotlightCaption = document.getElementById('spotlightCaption');
let activeIndex = 0;

function buildGallery() {
  photos.forEach((photo, index) => {
    const item = document.createElement('div');
    item.className = 'gallery-item';
    const img = document.createElement('img');
    img.src = photo.src;
    img.alt = photo.caption;
    img.dataset.index = index;
    img.loading = 'lazy';
    img.onclick = () => openSpotlight(index);
    item.appendChild(img);
    galleryGrid.appendChild(item);
  });
}

function openSpotlight(index) {
  activeIndex = index;
  updateSpotlight();
  spotlightOverlay.classList.add('active');
  spotlightOverlay.setAttribute('aria-hidden', 'false');
}

function closeSpotlight() {
  spotlightOverlay.classList.remove('active');
  spotlightOverlay.setAttribute('aria-hidden', 'true');
}

function updateSpotlight() {
  const photo = photos[activeIndex];
  spotlightImage.src = photo.src;
  spotlightImage.alt = photo.caption;
  spotlightCaption.textContent = photo.caption;
}

function prevPhoto() {
  activeIndex = (activeIndex - 1 + photos.length) % photos.length;
  updateSpotlight();
}

function nextPhoto() {
  activeIndex = (activeIndex + 1) % photos.length;
  updateSpotlight();
}

window.addEventListener('keydown', (event) => {
  if (!spotlightOverlay.classList.contains('active')) {
    return;
  }

  if (event.key === 'Escape') {
    closeSpotlight();
  }
  if (event.key === 'ArrowLeft') {
    prevPhoto();
  }
  if (event.key === 'ArrowRight') {
    nextPhoto();
  }
});

buildGallery();
</script>


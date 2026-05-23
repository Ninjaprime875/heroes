[photo_gallery_with_captions.md](https://github.com/user-attachments/files/28171005/photo_gallery_with_captions.md)---
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
  { src: "/images/gallery/aapi1.jpg", caption: "Dragon Youth Sports Program leaders and runners at the Belmont AAPI Run, celebrating youth athletics, cultural pride, and community leadership across generations." },
  { src: "/images/gallery/aapi2.jpg", caption: "Young participants gather at the starting line of the Belmont AAPI Run, showing the energy and teamwork behind a community event supporting AAPI youth in sports." },
  { src: "/images/gallery/aapi3.jpg", caption: "Dragon Youth Sports Program members and supporters celebrate at the Belmont AAPI Run finish line, honoring leadership, perseverance, and cross-cultural understanding." },
  { src: "/images/gallery/allstars.jpeg", caption: "AllStars Learning volunteers bring middle school and high school students together for peer mentorship, learning support, and community-centered education." },
  { src: "/images/gallery/bridge.jpg", caption: "Bridge School PTA volunteers and students celebrate curiosity at a school science fair, where young learners share experiments, discoveries, and the joy of science." },
  { src: "/images/gallery/bruce.jpeg", caption: "Bruce Leader portrays Lexington Minute Man Thaddeus Bowman on Patriots' Day, helping keep Lexington's Revolutionary history alive for the community." },
  { src: "/images/gallery/calex1.jpg", caption: "CALex volunteers, performers, elected officials, and community leaders gather after the 2026 Lunar New Year celebration, honoring the work of more than 100 volunteers and 200 performers." },
  { src: "/images/gallery/calex2.jpg", caption: "CALex and LexYouth recognize youth volunteer award winners for their dedication, service, and meaningful contributions to the Lexington community." },
  { src: "/images/gallery/counselor.jpeg", caption: "Munroe Arts camp counselors represent generations of creative learning at the Munroe Center for the Arts, a community institution shaped by imagination and hard work." },
  { src: "/images/gallery/display.png", caption: "At a community table in Depot Square, volunteers help visitors make crafts and take home a small piece of a larger shared celebration." },
  { src: "/images/gallery/farmers.jpeg", caption: "LexFarm volunteers gather produce from the farm's gleaning program to support local food security through donations to neighbors in need." },
  { src: "/images/gallery/festival1.jpg", caption: "At Lexington Lyceum's International Fun Fest, community members celebrate cultural diversity through Bollywood, Salsa, Chinese dance and song, and American jazz." },
  { src: "/images/gallery/festival2.jpg", caption: "New Legacy Culture Center volunteers and community partners prepare for the 2025 Lantern Festival promenade, bringing American historical tradition and Asian cultural expression together." },
  { src: "/images/gallery/festival3.jpg", caption: "Students and teachers celebrate Lunar New Year at Estabrook School, where culture becomes something to see, hear, feel, and share." },
  { src: "/images/gallery/festival4.jpg", caption: "At the NLCC Back-to-School Party, youth volunteers and Go Club instructors welcome families to learn strategy, culture, and connection one move at a time." },
  { src: "/images/gallery/festival5.jpg", caption: "The Shi Wei Book Club gathers at the Lexington Community Center, turning reading into a shared journey of reflection, conversation, and community." },
  { src: "/images/gallery/festival6.jpg", caption: "At the CAAL Lunar New Year Gala, grandmother and granddaughter share a Mongolian dance moment, passing heritage from one generation to the next." },
  { src: "/images/gallery/festival7.jpg", caption: "The opening dance of the CAAL Lunar New Year Gala welcomes the Year of the Horse with blossoms, movement, and galloping joy." },
  { src: "/images/gallery/festival8.jpg", caption: "Young dancers at the CAAL Lunar New Year Gala bring energy, beauty, and cultural blessings to the stage through a dance inspired by abundance." },
  { src: "/images/gallery/festival9.jpg", caption: "CAAL performers present Mulan, telling a story of bravery, devotion, and courage through dance." },
  { src: "/images/gallery/greenteam.png", caption: "LPS Green Team volunteers help students sort lunch waste into compost, recycling, trash, and food donations, turning daily habits into environmental stewardship." },
  { src: "/images/gallery/helpfulhands.jpg", caption: "Helpful Hands youth volunteers deliver clothing to the House of Hope shelter, continuing a five-year tradition of targeted service to neighbors in need." },
  { src: "/images/gallery/kolex.jpg", caption: "KOLex members, including children and teens, join the Patriots' Day Parade with pride, belonging, and appreciation for Lexington's history and community spirit." },
  { src: "/images/gallery/lexlux.jpg", caption: "At Lexington Town Diwali in Depot Square, community members gather for music, light, dance, colorful backdrops, and an illustration show with LexLux." },
  { src: "/images/gallery/musicfestival.jpg", caption: "Performers at the Asian Music Festival in Lexington share music as a bridge across cultures, generations, and community stories." },
  { src: "/images/gallery/musicians.jpeg", caption: "Mindful Musicians bring songs from the 1940s and 1950s to seniors at Artis Senior Living, creating joy, memory, and connection through music." },
  { src: "/images/gallery/parade1.jpeg", caption: "Munroe Center for the Arts volunteers bring public art into the Patriots' Day Parade, showing how many hands can turn civic tradition into shared creativity." },
  { src: "/images/gallery/parade2.jpg", caption: "CAAL waist drum performers debut a Chinese folk tradition in the Lexington Patriots' Day Parade after many evenings of practice and preparation." },
  { src: "/images/gallery/parade3.jpeg", caption: "The CAAL dragon team marches in the Patriots' Day Parade, adding color, movement, and cultural celebration to Lexington's historic streets." },
  { src: "/images/gallery/party.jpg", caption: "At a public light-and-community event in Depot Square, volunteers gather to brighten one of the darkest times of the year with warmth and creativity." },
  { src: "/images/gallery/pta.png", caption: "Harrington PTA volunteers and students celebrate Great Map Night, bringing families together for hands-on geography, crafts, and post-pandemic community learning." },
  { src: "/images/gallery/robot.jpg", caption: "Winsor Robotics students stand with their robot at a 2026 FTC qualifier, showing teamwork, engineering, problem-solving, and confidence in STEM." },
  { src: "/images/gallery/scouts1.jpg", caption: "Troop 119 scouts volunteer on Lexington conservation land, helping remove dead trees and clean trash in service of the town's natural spaces." },
  { src: "/images/gallery/scouts2.jpg", caption: "Cub Scout Pack 137 volunteers help LexFarm by clearing rocks from a field, turning small hands and steady effort into community service." },
  { src: "/images/gallery/scouts3.jpg", caption: "Troop 10 scouts support Lexington history and community service through projects ranging from Patriots' Day help to garden restoration and accessible outdoor spaces." },
  { src: "/images/gallery/volunteers.jpg", caption: "Community volunteers step forward in many forms — teaching, building, planting, performing, sorting, mentoring, and carrying Lexington's civic spirit into everyday life." }
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

---
title: "Photo Gallery"
description: "A Display of Community Submitted Photos of Heroes of Lexington"
featured_image: "/images/citizenheroes.png"
---

<div class="gallery-page">
  <p>Click any photo to spotlight it, then use the arrows or the close button to navigate.</p>
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
  { src: "/images/gallery/_Shadow Joy Lab- Shirley C 1.png", caption: "Over 24 families come together, sharing 36 free performances or workshops to thousands of audience members, preserving a 2000-year-old tradition of shadow puppetry." },
  { src: "/images/gallery/2026 World 2nd place - Jian Wen 1.jpg", caption: "A group of young future engineers celebrating their perserverance and dedication to STEM and Robotics at the world stage after receiving a great achievement." },
  { src: "/images/gallery/AllStars Learning - Mira Xu 1.jpg", caption: "Middle school and high school volunteers help host an event with over a dozen booths that appreciate the beauty of STEM." },
  { src: "/images/gallery/artwalk - Cristina Burwell 6.jpg", caption: "Artwalk volunteers come together, hosting a call-for-artists and placing meticulously curated submissions in windows to appreciate and promote local artists and retailers" },
  { src: "/images/gallery/BAA CEO Jack Flemming and Dragon Team Captain David Guo - David Guo 2.jpg", caption: "Two generations of leaders join hands to support youth in sports for AAPI, strengthening and celebrating their marvelous culture and community." },
  { src: "/images/gallery/bbp - Cristina Burwell 8.jpg", caption: "Volunteers staff a table for the Bikeway Block Party, allowing community members to create their own crafts to take home." },
  { src: "/images/gallery/Blooming - Mindful Musicians - Liyuan Ji 1.jpg", caption: "A resident gifts a young performer a plant, representing her appreciation for bringing the joy of music to her that brought back past memories." },
  { src: "/images/gallery/Bowman Green Team - Yiling Wang 6.png", caption: "The Bowman Elementary School Green Team, helping students throw away lunch in an eco-friendly and efficient way for the environment." },
  { src: "/images/gallery/Bridge_school_chess_team - Su Li 1.jpg", caption: "The Bridge School Chess Team competed and did a wonderful job at the Hurvitz cup chess tournament, demonstrating their resilliance and strength." },
  { src: "/images/gallery/Bridge_school_international_festival - Stephen Yang 1.jpg", caption: "The Bridge School PTA hosted a science fair for students, appreciating the beauty of the discoveries and experiments of science." },
  { src: "/images/gallery/Bridge_school_science_fair - Stephen Yang 2.jpg", caption: "The Bridge School held an international festival for families, giving the opportunity to share and appreciate the beauties of the culture of the Bridge community." },
  { src: "/images/gallery/CAAL - Melanie Lin 1.jpg", caption: "CAAL secretary Jian Deng participates not only as a volunteer for the AAPI race, but also ran the fastest in the race." },
  { src: "/images/gallery/CAAL - Melanie Lin 10.jpg", caption: "CAAL Board members perform "Fairytale of the Boat" for the audience at Lunar New Year." },
  { src: "/images/gallery/CAAL - Melanie Lin 11.jpg", caption: "Each year, some of the region's troupes perform at the CAAL Lunar Year Gala." },
  { src: "/images/gallery/CAAL - Melanie Lin 12.jpg", caption: "The audience of the CAAL Lunar New Year Gala, a full community appreciating the cultural beauty despite a snow day." },
  { src: "/images/gallery/CAAL - Melanie Lin 13.jpg", caption: "A record of over 600 runners and walkers at the AAPI 5k with a perfectly suiting theme of Better Together: From Lex250 to America250 },
  { src: "/images/gallery/CAAL - Melanie Lin 14.jpg", caption: "The Asian Music Festival is held in Lexington every two years, nicknamed "Tanglewood in Lexington". These are performers at the Asian Music Festival in 2024." },
  { src: "/images/gallery/CAAL - Melanie Lin 2.jpg", caption: "Over 100 volunteers supported the AAPI 5k Event of 2026." },
  { src: "/images/gallery/CAAL - Melanie Lin 3.jpg", caption: "A famous singer and Lexington resident Yan Lian sang the National Anthem at the AAPI 5K Run and Walk." },
  { src: "/images/gallery/CAAL - Melanie Lin 4.jpg", caption: "Lexington's AAPI 5K hosted by CAAL and HEY Runners Club marks the 5th year around and with a record of over 600 people attending, the theme of Better Together." },
  { src: "/images/gallery/CAAL - Melanie Lin 5.jpg", caption: "Volunteers provided Asian cultural food for runners and walkers at the AAPI 5K to appreciate and enjoy a taste of AAPI food." },
  { src: "/images/gallery/CAAL - Melanie Lin 6.jpg", caption: "Volunteers at the AAPI 5K manage recycle bins to run a green race for runners and walkers. " },
  { src: "/images/gallery/CAAL - Melanie Lin 7.jpg", caption: "CAAL's united group photo at the end of the Patriot's Day Parade." },
  { src: "/images/gallery/CAAL - Melanie Lin 8.jpg", caption: "The CAAL Dragon team plays dual dragons at the Patriots Day Parade." },
  { src: "/images/gallery/CAAL - Melanie Lin 9.jpg", caption: "CAAL organizes the annual Lunar New Year celebration for the town of Lexington, quoted as the best lunar new year performance in the region." },
  { src: "/images/gallery/CAAL - Qian Hu 1.jpg", caption: "Two dancers dressed in marvelous Mongolian costumes perform for the crowd, one gifting a silver bowl to her grandmother, a symbol of respect --- representing the beauty of a generation passing to another." },
  { src: "/images/gallery/CAAL - Qian Hu 2.jpg", caption: "The opening dance of the 2026 CAAL Lunar New Year, ushering in the year of the Horse, depicting blossoms of flowers and lively, galloping horses." },
  { src: "/images/gallery/CAAL - Qian Hu 3.jpg", caption: "A busy group of young girls went through many, many performances on the same day yet still put a beautiful show for the audience. Their energetic dance movements representing "Fish", a homophone in Chinese for abundance, captivated the audience and brought them a deep, cultural message of good wish" },
  { src: "/images/gallery/CAAL - Qian Hu 4.jpg", caption: "The dance of Mulan tells the story of a young girl who sacrificed her youth to help her father in the army, a story of courage." },
  { src: "/images/gallery/CAAL - Qian Hu 5.jpg", caption: "The debut of a Chinese folk waist drum performance at the Lexington 2026 Patriots Day Parade, spending much time and effort to prepare." },
  { src: "/images/gallery/CALex Youth Book Club  - Yuning Song 1.jpg", caption: "The first elementary student-run club in town, creating their own Jeopardy questions, games, exploring reading and science, and donating to the community center every Thanksgiving." },
  { src: "/images/gallery/CaLexLunarNewYear2026 - Houze Xu 1.jpg", caption: "Caption placeholder" },
  { src: "/images/gallery/CaLexLunarNewYear2026 2 - Houze Xu 2.jpg", caption: "Caption placeholder" },
  { src: "/images/gallery/CALL- Culture Bridge - Anne Zhang 1.jpg", caption: "Caption placeholder" },
  { src: "/images/gallery/camp counselrs - Cristina Burwell 9.jpg", caption: "Caption placeholder" },
  { src: "/images/gallery/citizen heroes - Mark Manasas 1.jpg", caption: "Caption placeholder" },
  { src: "/images/gallery/Clarke green team - Yiling Wang 4.png", caption: "Caption placeholder" },
  { src: "/images/gallery/Cub Scouts Pack 137 - Jing Zheng 2.jpeg", caption: "Caption placeholder" },
  { src: "/images/gallery/Diamond green team - Yiling Wang 5.png", caption: "Caption placeholder" },
  { src: "/images/gallery/Dragon Program Chair Lixin Qin and Dragon Team Captain David Guo with Fellow Dragon Team Members and Supporters at Belmont Finish Line - David Guo 5.jpg", caption: "Caption placeholder" },
  { src: "/images/gallery/Dragon Team Captains William Qin, David Guo and Supporter Qian Ge (NECAA Chair) - David Guo 3.jpg", caption: "Caption placeholder" },
  { src: "/images/gallery/Dragon Team Chair Lixin Qin and Head Coach Heidi - David Guo 1.jpg", caption: "Caption placeholder" },
  { src: "/images/gallery/Excellent Youth Participation Inspired by Dragon Team Initiatives and Well Organized Activities - David Guo 4.jpg", caption: "Caption placeholder" },
  { src: "/images/gallery/hands - Cristina Burwell 7.jpeg", caption: "Caption placeholder" },
  { src: "/images/gallery/Harrington Green Team - Yiling Wang 1.png", caption: "Caption placeholder" },
  { src: "/images/gallery/Helpful Hands - Justin Wang 2.jpg", caption: "Caption placeholder" },
  { src: "/images/gallery/Helpful Hands - Justin Wang 3.jpg", caption: "Caption placeholder" },
  { src: "/images/gallery/Helpful Hands delivery - Justin Wang 1.jpg", caption: "Caption placeholder" },
  { src: "/images/gallery/hope - Cristina Burwell 5.JPG", caption: "Caption placeholder" },
  { src: "/images/gallery/horse - Cristina Burwell 3.jpeg", caption: "Caption placeholder" },
  { src: "/images/gallery/International Fun Fest - Mark Manasas 3.jpeg", caption: "Caption placeholder" },
  { src: "/images/gallery/International Fun Fest - Mark Manasas 5.jpg", caption: "Caption placeholder" },
  { src: "/images/gallery/Juliana Yin Winning the Age Group Championship at Belmont AAPI 5k Run Finish Line - David Guo 6.jpg", caption: "Caption placeholder" },
  { src: "/images/gallery/KOLex - Youngsheen Park 1.jpg", caption: "Caption placeholder" },
  { src: "/images/gallery/Lafayette Returns  - Harry Forsdick 1.png", caption: "Caption placeholder" },
  { src: "/images/gallery/LexFarm - Sara Bothwell Allen 1.jpeg", caption: "Caption placeholder" },
  { src: "/images/gallery/Lexington Field & Garden Club - Regina Sutton 1.jpg", caption: "Caption placeholder" },
  { src: "/images/gallery/Lexington Field & Garden Club - Regina Sutton 3.jpg", caption: "Caption placeholder" },
  { src: "/images/gallery/Lexington Field & Garden Club - Regina Sutton 4.jpg", caption: "Caption placeholder" },
  { src: "/images/gallery/Lexington Field & Garden Club - Regina Sutton 5.jpg", caption: "Caption placeholder" },
  { src: "/images/gallery/Lexington Field & Garden Club - Regina Sutton 6.jpg", caption: "Caption placeholder" },
  { src: "/images/gallery/Lexington Field & Garden Club - Regina Sutton 9.jpg", caption: "Caption placeholder" },
  { src: "/images/gallery/Lexington Field & Garden Club- Regina Sutton 2.jpg", caption: "Caption placeholder" },
  { src: "/images/gallery/Lexington Field & Garden Club- Regina Sutton 7.jpg", caption: "Caption placeholder" },
  { src: "/images/gallery/Lexington Field & Garden Club- Regina Sutton 8.jpg", caption: "Caption placeholder" },
  { src: "/images/gallery/lexlux - Cristina Burwell 4.jpg", caption: "Caption placeholder" },
  { src: "/images/gallery/Luckie Art Students Volunteers - Yue Zheng 1.jpg", caption: "Caption placeholder" },
  { src: "/images/gallery/Lyceum calendar turning - Mark Manasas 4.png", caption: "Caption placeholder" },
  { src: "/images/gallery/Mrs. Carpenter and CCC kids - Yiling Wang 3.png", caption: "Caption placeholder" },
  { src: "/images/gallery/NLCC - Elizabeth Xu 1.jpg", caption: "Caption placeholder" },
  { src: "/images/gallery/NLCC - Elizabeth Xu 2.jpg", caption: "Caption placeholder" },
  { src: "/images/gallery/NLCC - Elizabeth Xu 3.jpg", caption: "Caption placeholder" },
  { src: "/images/gallery/NLCC - Elizabeth Xu 4.jpg", caption: "Caption placeholder" },
  { src: "/images/gallery/Pack 160 - Charles Miglietti 1.jpg", caption: "Caption placeholder" },
  { src: "/images/gallery/parade - Cristina Burwell 1.jpeg", caption: "Caption placeholder" },
  { src: "/images/gallery/PARADE 2 - Cristina Burwell 2.jpeg", caption: "Caption placeholder" },
  { src: "/images/gallery/porchfest - Cristina Burwell 10.jpeg", caption: "Caption placeholder" },
  { src: "/images/gallery/PTA leaders hosting events_2023 - Yiling Wang 2.png", caption: "Caption placeholder" },
  { src: "/images/gallery/Rainbow HLDC Bedford - Lili Feng.jpg", caption: "Caption placeholder" },
  { src: "/images/gallery/Reenactments 2 - Bruce Leader 2.jpeg", caption: "Caption placeholder" },
  { src: "/images/gallery/Reenactments- Bruce Leader 1.jpeg", caption: "Caption placeholder" },
  { src: "/images/gallery/Scibowl - Zhengjia Yang 1.png", caption: "Caption placeholder" },
  { src: "/images/gallery/Shadow Joy Lab - Liang Cao 1.jpg", caption: "Caption placeholder" },
  { src: "/images/gallery/Shadow Joy Lab - Liang Cao 2.jpg", caption: "Caption placeholder" },
  { src: "/images/gallery/Shadow Joy Lab - Liang Cao 3.jpg", caption: "Caption placeholder" },
  { src: "/images/gallery/Troop 10 - Charles Miglietti 2.jpg", caption: "Caption placeholder" },
  { src: "/images/gallery/Troop 10 - Chip Webb 1.jpg", caption: "Caption placeholder" },
  { src: "/images/gallery/Troop 10 - Chip Webb 2.jpg", caption: "Caption placeholder" },
  { src: "/images/gallery/Troop 10 - Chip Webb 3.jpg", caption: "Caption placeholder" },
  { src: "/images/gallery/Troop 10 - Chip Webb 4.jpg", caption: "Caption placeholder" },
  { src: "/images/gallery/Troop 10 - Chip Webb 5.jpg", caption: "Caption placeholder" },
  { src: "/images/gallery/Troop 10 - Chip Webb 6.jpg", caption: "Caption placeholder" },
  { src: "/images/gallery/Troop 10 - Chip Webb 7.jpg", caption: "Caption placeholder" },
  { src: "/images/gallery/Troop 10 - Chip Webb 9.jpg", caption: "Caption placeholder" },
  { src: "/images/gallery/Troop 119 - Jing Zheng 1.jpg", caption: "Caption placeholder" },
  { src: "/images/gallery/VO250 - Sara Bothwell Allen 2.jpeg", caption: "Caption placeholder" },
  { src: "/images/gallery/VOID Robotics - Grace Fan 1.jpeg", caption: "Caption placeholder" },
  { src: "/images/gallery/William Diamond Jr Fife & Drum - Chip Webb 8.jpg", caption: "Caption placeholder" },
  { src: "/images/gallery/Winsor Robotics - Lucy Yin 1.jpg", caption: "Caption placeholder" },
  { src: "/images/gallery/Winsor Robotics - Lucy Yin 2.jpg", caption: "Caption placeholder" },
  { src: "/images/gallery/Winsor Robotics - Lucy Yin 3.jpg", caption: "Caption placeholder" },
  { src: "/images/gallery/Winsor Robotics - Lucy Yin 5.JPG", caption: "Caption placeholder" },
  { src: "/images/gallery/Winsor Robotics - Lucy Yin 6.JPG", caption: "Caption placeholder" },
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

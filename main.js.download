// Smooth scroll
document.querySelectorAll('a[href^="#"]').forEach(anchor=>{
  anchor.addEventListener('click', function(e){
    e.preventDefault();
    document.querySelector(this.getAttribute('href')).scrollIntoView({behavior:'smooth'});
  });
});

// Scroll animations
const faders = document.querySelectorAll('.fade-up, .slide-left, .slide-right');
const appearOptions = {threshold:0.1, rootMargin:"0px 0px -50px 0px"};

const appearOnScroll = new IntersectionObserver(function(entries, appearOnScroll){
  entries.forEach(entry=>{
    if(!entry.isIntersecting) return;
    entry.target.style.animationPlayState='running';
    appearOnScroll.unobserve(entry.target);
  });
}, appearOptions);

faders.forEach(fader => appearOnScroll.observe(fader));

// Expertise caret toggle: move lab-cards to top and scroll into view
const expertiseToggle = document.getElementById('expertise-toggle');
if (expertiseToggle) {
  expertiseToggle.addEventListener('click', function(e){
    e.preventDefault();
    const expertise = document.getElementById('expertise');
    if(!expertise) return;
      // Always show cards; clicking Expertise just scrolls and focuses the first card
    expertiseToggle.classList.add('active');
    expertiseToggle.setAttribute('aria-expanded', 'true');
    const cards = expertise.querySelector('.lab-cards');
    if(cards){ cards.classList.add('flash'); setTimeout(()=> cards.classList.remove('flash'), 900); }
    expertise.scrollIntoView({ behavior: 'smooth', block: 'start' });
    const firstCard = expertise.querySelector('.lab-cards .card');
    if(firstCard){
      firstCard.focus();
      setTimeout(()=>{ firstCard.scrollIntoView({ behavior: 'smooth', block: 'center' }); }, 260);
    }
  });
}

// Ensure any other link to #expertise (not the main toggle) shows the cards and scrolls there
document.querySelectorAll('a[href="#expertise"]:not(#expertise-toggle)').forEach(link => {
  link.addEventListener('click', function(e){
    e.preventDefault();
    const expertise = document.getElementById('expertise');
    if(!expertise) return;
    const toggle = document.getElementById('expertise-toggle');
    if(toggle) { toggle.classList.add('active'); toggle.setAttribute('aria-expanded', 'true'); setTimeout(()=> toggle.classList.remove('active'), 800); }
    const cards = expertise.querySelector('.lab-cards');
    if(cards){ cards.classList.add('flash'); setTimeout(()=> cards.classList.remove('flash'), 900); }
    expertise.scrollIntoView({ behavior: 'smooth' });
    const firstCard = expertise.querySelector('.lab-cards .card');
    if(firstCard) { firstCard.focus(); setTimeout(()=>{ firstCard.scrollIntoView({ behavior: 'smooth', block: 'center' }); }, 200); }
  });
});

// If page loads with #expertise in URL, open the cards
window.addEventListener('load', function(){
  if(window.location.hash === '#expertise'){
    const expertise = document.getElementById('expertise');
    if(expertise){
      const toggle = document.getElementById('expertise-toggle');
      if(toggle) { toggle.classList.add('active'); toggle.setAttribute('aria-expanded', 'true'); setTimeout(()=> toggle.classList.remove('active'), 900); }
      const cards = expertise.querySelector('.lab-cards');
      if(cards){ cards.classList.add('flash'); setTimeout(()=> cards.classList.remove('flash'), 900); }
      setTimeout(()=>{ expertise.scrollIntoView({ behavior: 'smooth' }); const firstCard = expertise.querySelector('.lab-cards .card'); if(firstCard) firstCard.focus(); }, 100);
    }
  }

  // Hero parallax: update --hero-translate based on scroll
  const hero = document.querySelector('.hero');
  if(hero){
    let latestKnownScrollY = 0; let ticking = false;
    function onScroll(){ latestKnownScrollY = window.scrollY; requestTick(); }
    function requestTick(){ if(!ticking){ requestAnimationFrame(updatePositions); ticking = true; } }
    function updatePositions(){ const translate = Math.max(-60, Math.min(60, -latestKnownScrollY * 0.04)); hero.style.setProperty('--hero-translate', translate + 'px'); ticking = false; }
    window.addEventListener('scroll', onScroll, { passive: true });
    updatePositions();
  }

  // Mobile navigation toggle
  const menuToggle = document.querySelector('.menu-toggle');
  const mainNav = document.getElementById('main-nav');
  if(menuToggle && mainNav){
    menuToggle.addEventListener('click', () => {
      const expanded = menuToggle.getAttribute('aria-expanded') === 'true';
      menuToggle.setAttribute('aria-expanded', String(!expanded));
      mainNav.classList.toggle('open');
      menuToggle.classList.toggle('open');
    });
    // close nav when a link is clicked (good for mobile)
    mainNav.querySelectorAll('a').forEach(a => a.addEventListener('click', () => {
      if(mainNav.classList.contains('open')){ mainNav.classList.remove('open'); menuToggle.setAttribute('aria-expanded', 'false'); menuToggle.classList.remove('open'); }
    }));
    // close on Escape
    document.addEventListener('keydown', (e) => {
      if(e.key === 'Escape' && mainNav.classList.contains('open')){ mainNav.classList.remove('open'); menuToggle.setAttribute('aria-expanded', 'false'); menuToggle.classList.remove('open'); menuToggle.focus(); }
    });
  }

  // Section observer sets active nav link based on viewport
  const sections = document.querySelectorAll('main section[id]');
  const sectionObserver = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if(!entry.isIntersecting) return;
      const id = entry.target.id;
      document.querySelectorAll('nav a').forEach(a => a.classList.remove('active'));
      const link = document.querySelector(`nav a[href="#${id}"]`);
      if(link) link.classList.add('active');
      // briefly highlight hero when home becomes active
      if(id === 'home' && typeof hero !== 'undefined' && hero){
        hero.classList.add('hero--focus');
        setTimeout(() => hero.classList.remove('hero--focus'), 2600);
      }
    });
  }, { threshold: 0.55, rootMargin: ' -10% 0% -30% 0%'});
  sections.forEach(s => sectionObserver.observe(s));

  // Home click should focus hero and set active link immediately
  document.querySelectorAll('a[href="#home"]').forEach(hl => hl.addEventListener('click', () => {
    const navHome = document.querySelector('nav a[href="#home"]');
    if(navHome){ document.querySelectorAll('nav a').forEach(a=> a.classList.remove('active')); navHome.classList.add('active'); }
    if(hero){ hero.classList.add('hero--focus'); setTimeout(() => hero.classList.remove('hero--focus'), 2600); }
  }));

  // Update footer year to current year
  const yearEl = document.getElementById('year'); if(yearEl){ yearEl.textContent = new Date().getFullYear(); }

});

// ==========================================================
// PAUL CHINNU — Portfolio interactions
// ==========================================================

document.addEventListener('DOMContentLoaded', () => {

  /* ---------- Preloader ---------- */
  const preloader = document.getElementById('preloader');
  const countEl = document.getElementById('preloaderCount');
  const enterBtn = document.getElementById('preloaderEnter');
  let pct = 0;

  const tick = setInterval(() => {
    pct += Math.floor(Math.random() * 12) + 4;
    if (pct >= 100) {
      pct = 100;
      clearInterval(tick);
      enterBtn.classList.add('visible');
    }
    countEl.textContent = pct + '%';
  }, 120);

  function closePreloader() {
    preloader.classList.add('hidden');
    document.body.style.overflow = '';
  }
  enterBtn.addEventListener('click', closePreloader);
  // Fallback: auto-dismiss shortly after full load in case the user doesn't click
  window.addEventListener('load', () => {
    setTimeout(() => {
      if (pct >= 100) enterBtn.classList.add('visible');
    }, 400);
  });

  /* ---------- Mobile nav ---------- */
  const navToggle = document.getElementById('navToggle');
  const mainNav = document.getElementById('mainNav');
  navToggle.addEventListener('click', () => mainNav.classList.toggle('open'));
  mainNav.querySelectorAll('a').forEach(a =>
    a.addEventListener('click', () => mainNav.classList.remove('open'))
  );

  /* ---------- Services data ---------- */
  const services = [
    { num: '01', title: 'Digital Marketing', desc: 'Multi-channel strategy & digital presence growth.', img: 'assets/Digitalmarketing.png' },
    { num: '02', title: 'Google Ads', desc: 'Precision search & video ad campaign management.', img: 'assets/GoogleAds.png' },
    { num: '03', title: 'Meta Ads', desc: 'Facebook & Instagram ad funnel optimization.', img: 'assets/metaAds.png' },
    { num: '04', title: 'Social Media Marketing', desc: 'Organic social strategy & community growth.', img: 'assets/Social.png' },
    { num: '05', title: 'Graphic Design', desc: 'Promotional graphics & advertising artwork.', img: 'assets/GraphicDesign.png' },
    { num: '06', title: 'Video Editing', desc: 'Promotional videos, reels & cinematic edits.', img: 'assets/video.png' },
    { num: '07', title: 'Lead Generation', desc: 'Targeted acquisition funnels & email/WhatsApp flows.', img: 'assets/Lead.png' },
    { num: '08', title: 'Branding & Creative Content', desc: 'Brand identity system & creative asset design.', img: 'assets/Branding.png' },
    { num: '09', title: 'WordPress Websites', desc: 'Custom, responsive landing pages & portals.', img: 'assets/Wordpress.png' },
  ];

  const servicesGrid = document.getElementById('servicesGrid');
  servicesGrid.innerHTML = services.map(s => `
    <article class="service-card">
      <div class="service-thumb"><img src="${s.img}" alt="${s.title}" loading="lazy"></div>
      <div class="service-body">
        <p class="service-num">${s.num}</p>
        <h3>${s.title}</h3>
        <p>${s.desc}</p>
        <span class="service-link">Explore Service</span>
      </div>
    </article>
  `).join('');

  /* ---------- Work data ---------- */
  const projects = [
    { title: 'Multi-Channel Digital Marketing Strategy', cat: 'Digital Marketing & Growth', filter: 'ads', img: 'assets/D1.png' },
    { title: 'Technovate Global Campaign', cat: 'Digital Advertisements', filter: 'ads', img: 'assets/work-ad.jpg' },
    { title: 'Paul Chinnu Studio Brand Art', cat: 'Branding & Creative', filter: 'branding', img: 'assets/paul-hero.jpg' },
    { title: 'Cinematic Promo Reel Editing', cat: 'Video Editing Projects', filter: 'video', img: 'assets/work-video.jpg' },
    { title: 'Institutional Campaign Graphic', cat: 'Social Media Creatives', filter: 'social', img: 'assets/work-ad.jpg' },
  ];

  const workGrid = document.getElementById('workGrid');
  workGrid.innerHTML = projects.map((p, i) => `
    <article class="work-card" data-filter="${p.filter}" data-index="${i}">
      <div class="work-thumb"><img src="${p.img}" alt="${p.title}" loading="lazy"></div>
      <div class="work-info">
        <p class="work-cat">${p.cat}</p>
        <h3>${p.title}</h3>
      </div>
    </article>
  `).join('');

  /* ---------- Work filtering ---------- */
  const filterBtns = document.querySelectorAll('.filter-btn');
  const workCards = document.querySelectorAll('.work-card');

  filterBtns.forEach(btn => {
    btn.addEventListener('click', () => {
      filterBtns.forEach(b => b.classList.remove('active'));
      btn.classList.add('active');
      const filter = btn.dataset.filter;
      workCards.forEach(card => {
        const match = filter === 'all' || card.dataset.filter === filter;
        card.hidden = !match;
      });
    });
  });

  /* ---------- Project modal ---------- */
  const modal = document.getElementById('projectModal');
  const modalImg = document.getElementById('modalImg');
  const modalCat = document.getElementById('modalCat');
  const modalTitle = document.getElementById('modalTitle');
  const modalClose = document.getElementById('modalClose');
  const modalBackdrop = document.getElementById('modalBackdrop');

  function openModal(project) {
    modalImg.src = project.img;
    modalImg.alt = project.title;
    modalCat.textContent = project.cat;
    modalTitle.textContent = project.title;
    modal.classList.add('open');
    modal.setAttribute('aria-hidden', 'false');
    document.body.style.overflow = 'hidden';
  }
  function closeModal() {
    modal.classList.remove('open');
    modal.setAttribute('aria-hidden', 'true');
    document.body.style.overflow = '';
  }

  document.querySelectorAll('.work-card').forEach(card => {
    card.addEventListener('click', () => {
      const i = Number(card.dataset.index);
      openModal(projects[i]);
    });
  });
  modalClose.addEventListener('click', closeModal);
  modalBackdrop.addEventListener('click', closeModal);
  document.addEventListener('keydown', e => {
    if (e.key === 'Escape') closeModal();
  });

  /* ---------- Contact form (client-side only, no backend) ---------- */
  const form = document.getElementById('contactForm');
  const status = document.getElementById('formStatus');

  form.addEventListener('submit', e => {
    e.preventDefault();
    const name = form.name.value.trim();
    const email = form.email.value.trim();
    const message = form.message.value.trim();

    if (!name || !email || !message) {
      status.textContent = 'Please fill in every field.';
      return;
    }

    // No backend wired up — open the user's mail client with the details prefilled.
    const subject = encodeURIComponent(`Project inquiry from ${name}`);
    const body = encodeURIComponent(`${message}\n\n— ${name} (${email})`);
    window.location.href = `mailto:chinnudhanush25@gmail.com?subject=${subject}&body=${body}`;

    status.textContent = 'Opening your email client…';
    form.reset();
  });

  /* ---------- Active nav link on scroll ---------- */
  const sections = document.querySelectorAll('section[id]');
  const navLinks = document.querySelectorAll('.main-nav a');

  const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        const id = entry.target.getAttribute('id');
        navLinks.forEach(link => {
          link.style.color = link.getAttribute('href') === `#${id}` ? 'var(--paper)' : '';
        });
      }
    });
  }, { rootMargin: '-50% 0px -45% 0px' });

  sections.forEach(sec => observer.observe(sec));
});

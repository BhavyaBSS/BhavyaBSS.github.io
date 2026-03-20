/* ============================================================
   PORTFOLIO — script.js
   Author: Your Name
   
   TABLE OF CONTENTS:
   1. Dark/Light Theme Toggle
   2. Navbar: Scroll Behaviour + Active Link
   3. Mobile Hamburger Menu
   4. Scroll-Reveal Animation (IntersectionObserver)
   5. Skill Bar Animation (IntersectionObserver)
   6. Active Nav Link Highlight on Scroll (Spy)
   7. Contact Form Submission Handler
   8. Smooth Anchor Scroll (for older browser fallback)
   9. Init — Call everything on DOMContentLoaded
============================================================ */


/* ============================================================
   1. THEME TOGGLE
   Switches between 'dark' (default) and 'light' themes by
   toggling the data-theme attribute on <html>.
   Persists the preference to localStorage.
============================================================ */
function initThemeToggle() {
  const html   = document.documentElement;
  const toggle = document.getElementById('themeToggle');

  // Load saved preference (default: dark)
  const savedTheme = localStorage.getItem('portfolio-theme') || 'dark';
  html.setAttribute('data-theme', savedTheme);

  toggle.addEventListener('click', () => {
    const current = html.getAttribute('data-theme');
    const next    = current === 'dark' ? 'light' : 'dark';
    html.setAttribute('data-theme', next);
    localStorage.setItem('portfolio-theme', next);
  });
}


/* ============================================================
   2. NAVBAR SCROLL BEHAVIOUR
   Adds .scrolled class to navbar after user scrolls 60px.
   This triggers the frosted-glass background via CSS.
============================================================ */
function initNavbar() {
  const navbar = document.getElementById('navbar');

  function onScroll() {
    if (window.scrollY > 60) {
      navbar.classList.add('scrolled');
    } else {
      navbar.classList.remove('scrolled');
    }
  }

  window.addEventListener('scroll', onScroll, { passive: true });
  onScroll(); // Run once on load in case page is refreshed mid-scroll
}


/* ============================================================
   3. MOBILE HAMBURGER MENU
   Toggles the .open class on both the button and nav-links.
   Closes the menu when any nav link is clicked.
============================================================ */
function initHamburger() {
  const hamburger = document.getElementById('hamburger');
  const navLinks  = document.getElementById('nav-links');

  hamburger.addEventListener('click', () => {
    hamburger.classList.toggle('open');
    navLinks.classList.toggle('open');
    // Prevent background scroll while menu is open
    document.body.style.overflow = navLinks.classList.contains('open') ? 'hidden' : '';
  });

  // Close menu on any link click
  navLinks.querySelectorAll('a').forEach(link => {
    link.addEventListener('click', () => {
      hamburger.classList.remove('open');
      navLinks.classList.remove('open');
      document.body.style.overflow = '';
    });
  });
}


/* ============================================================
   4. SCROLL-REVEAL ANIMATION
   Watches for elements with class .reveal. When they enter
   the viewport, adds .visible which triggers the CSS fade-up.
   Uses IntersectionObserver for performance (no scroll events).
============================================================ */
function initScrollReveal() {
  const revealEls = document.querySelectorAll('.reveal');

  if (!revealEls.length) return;

  const observer = new IntersectionObserver(
    (entries) => {
      entries.forEach(entry => {
        if (entry.isIntersecting) {
          entry.target.classList.add('visible');
          // Once revealed, stop observing to save resources
          observer.unobserve(entry.target);
        }
      });
    },
    {
      threshold: 0.12,      // Trigger when 12% of element is visible
      rootMargin: '0px 0px -40px 0px' // Trigger slightly before element enters
    }
  );

  revealEls.forEach(el => observer.observe(el));
}


/* ============================================================
   5. SKILL BAR ANIMATION
   Finds all .skill-fill elements. When they enter the viewport,
   reads their data-width attribute and animates the CSS width.
   This creates the filling bar effect on scroll.
============================================================ */
function initSkillBars() {
  const skillFills = document.querySelectorAll('.skill-fill');

  if (!skillFills.length) return;

  const observer = new IntersectionObserver(
    (entries) => {
      entries.forEach(entry => {
        if (entry.isIntersecting) {
          const targetWidth = entry.target.getAttribute('data-width');
          // Small delay so the reveal animation plays first
          setTimeout(() => {
            entry.target.style.width = targetWidth + '%';
          }, 200);
          observer.unobserve(entry.target);
        }
      });
    },
    { threshold: 0.3 }
  );

  skillFills.forEach(el => observer.observe(el));
}


/* ============================================================
   6. ACTIVE NAV LINK HIGHLIGHT (SCROLL SPY)
   As the user scrolls, highlights the corresponding nav link
   for whichever section is currently in view.
============================================================ */
function initScrollSpy() {
  // All nav links that point to section IDs
  const navLinks = document.querySelectorAll('.nav-link[href^="#"]');
  
  // Map href → link element for quick lookup
  const linkMap = {};
  navLinks.forEach(link => {
    linkMap[link.getAttribute('href').slice(1)] = link;
  });

  // All sections that have a corresponding nav link
  const sectionIds = Array.from(navLinks)
    .map(l => l.getAttribute('href').slice(1))
    .filter(id => document.getElementById(id));

  function onScroll() {
    const scrollY = window.scrollY + window.innerHeight * 0.35;

    let activeId = sectionIds[0];

    sectionIds.forEach(id => {
      const el = document.getElementById(id);
      if (!el) return;
      if (el.offsetTop <= scrollY) {
        activeId = id;
      }
    });

    // Remove active from all, set on current
    navLinks.forEach(l => l.classList.remove('active'));
    if (linkMap[activeId]) {
      linkMap[activeId].classList.add('active');
    }
  }

  window.addEventListener('scroll', onScroll, { passive: true });
  onScroll(); // Run once on load
}


/* ============================================================
   7. CONTACT FORM HANDLER
   Basic form submission handler. Since this is a static site,
   it simulates a submission with a success message.
   
   TO CONNECT A REAL BACKEND:
   - Replace the setTimeout block with a fetch() call to
     your API endpoint (e.g. Formspree, EmailJS, or your own).
   - Example: fetch('https://formspree.io/f/YOUR_ID', { method:'POST', body: new FormData(form) })
============================================================ */
function initContactForm() {
  const form     = document.getElementById('contactForm');
  const noteEl   = document.getElementById('formNote');

  if (!form) return;

  form.addEventListener('submit', async (e) => {
    e.preventDefault();

    // Get form values
    const name    = form.name.value.trim();
    const email   = form.email.value.trim();
    const message = form.message.value.trim();

    // Basic validation
    if (!name || !email || !message) {
      showNote('Please fill in all required fields.', 'error');
      return;
    }

    // Email format check
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    if (!emailRegex.test(email)) {
      showNote('Please enter a valid email address.', 'error');
      return;
    }

    // Disable button to prevent double submit
    const submitBtn = form.querySelector('button[type="submit"]');
    submitBtn.disabled = true;
    submitBtn.textContent = 'Sending...';

    // --- REPLACE THIS BLOCK WITH REAL API CALL ---
    // Simulating a network request with setTimeout
    await new Promise(resolve => setTimeout(resolve, 1500));
    // --------------------------------------------

    // Show success
    form.reset();
    submitBtn.disabled = false;
    submitBtn.textContent = 'Send Message →';
    showNote('✓ Message sent! I\'ll get back to you within 24 hours.', 'success');

    // Clear note after 5 seconds
    setTimeout(() => { noteEl.textContent = ''; }, 5000);
  });

  function showNote(msg, type) {
    noteEl.textContent = msg;
    noteEl.style.color = type === 'error' ? '#e85a4f' : 'var(--accent)';
  }
}


/* ============================================================
   8. SMOOTH ANCHOR SCROLL (Fallback for older Safari)
   CSS scroll-behavior: smooth works in all modern browsers,
   but this JS fallback handles edge cases with fixed navbar offset.
============================================================ */
function initSmoothScroll() {
  const navbarHeight = document.getElementById('navbar').offsetHeight;

  document.querySelectorAll('a[href^="#"]').forEach(link => {
    link.addEventListener('click', (e) => {
      const targetId = link.getAttribute('href').slice(1);
      if (!targetId) return; // skip plain '#' links

      const target = document.getElementById(targetId);
      if (!target) return;

      e.preventDefault();

      const targetTop = target.getBoundingClientRect().top + window.scrollY - navbarHeight - 16;

      window.scrollTo({
        top: targetTop,
        behavior: 'smooth'
      });
    });
  });
}


/* ============================================================
   9. TYPING EFFECT (Optional enhancement)
   Adds a subtle typing cursor blink effect to the hero title.
   Purely cosmetic — feel free to remove if not needed.
============================================================ */
function initHeroTyping() {
  const heroTitle = document.querySelector('.hero-name');
  if (!heroTitle) return;

  // Add a blinking cursor after the name
  const cursor = document.createElement('span');
  cursor.textContent = '_';
  cursor.style.cssText = `
    display: inline-block;
    color: var(--accent);
    font-weight: 300;
    margin-left: 2px;
    animation: blink 1.1s step-end infinite;
  `;

  // Add blink keyframe to document
  const style = document.createElement('style');
  style.textContent = `
    @keyframes blink {
      0%, 100% { opacity: 1; }
      50% { opacity: 0; }
    }
  `;
  document.head.appendChild(style);
  heroTitle.appendChild(cursor);
}


/* ============================================================
   INIT — Run all functions after DOM is fully loaded
============================================================ */
document.addEventListener('DOMContentLoaded', () => {
  initThemeToggle();    // Theme toggle (dark/light)
  initNavbar();         // Navbar scroll glass effect
  initHamburger();      // Mobile menu
  initScrollReveal();   // Fade-up reveal on scroll
  initSkillBars();      // Skill bar fill animation
  initScrollSpy();      // Active nav link highlighting
  initContactForm();    // Contact form handler
  initSmoothScroll();   // Smooth anchor scroll with offset
  initHeroTyping();     // Blinking cursor on hero name (optional)

  console.log('%c Portfolio loaded ✓', 'color: #e8a427; font-size: 14px; font-weight: bold;');
});

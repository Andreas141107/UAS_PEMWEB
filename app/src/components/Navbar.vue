<script setup>
import { ref, watch } from 'vue';
import { useRoute, useRouter } from 'vue-router';

const route = useRoute();
const router = useRouter();

const isScrolled = ref(false);
const mobileMenuOpen = ref(false);

const navLinks = [
  { name: 'Home', path: '/' },
  { name: 'Explore', path: '/explore' },
  { name: 'Gallery', path: '/gallery' },
  { name: 'Trails', path: '/trails' },
  { name: 'Contact', path: '/contact' }
];

// Scroll handler
const handleScroll = () => {
  isScrolled.value = window.scrollY > 100;
};

window.addEventListener('scroll', handleScroll);

// Mobile menu toggle
const toggleMobileMenu = () => {
  mobileMenuOpen.value = !mobileMenuOpen.value;
};

// Close mobile menu on route change
watch(() => route.path, () => {
  mobileMenuOpen.value = false;
});

const navigateTo = (path) => {
  router.push(path);
  mobileMenuOpen.value = false;
};
</script>

<template>
  <nav class="navbar" :class="{ 'navbar--scrolled': isScrolled }">
    <div class="navbar__container container">
      <!-- Logo -->
      <router-link to="/" class="navbar__logo">
        <svg class="navbar__logo-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
          <path d="M8 16L12 8L16 16M12 12V20M4 20H20" stroke-linecap="round" stroke-linejoin="round"/>
        </svg>
        <span class="navbar__logo-text">Gede Pangrango</span>
      </router-link>

      <!-- Desktop Nav Links -->
      <div class="navbar__links">
        <router-link
          v-for="link in navLinks"
          :key="link.path"
          :to="link.path"
          class="navbar__link"
          :class="{ 'navbar__link--active': route.path === link.path }"
        >
          {{ link.name }}
        </router-link>
      </div>

      <!-- CTA + Hamburger -->
      <div class="navbar__actions">
        <router-link to="/explore" class="btn btn-primary navbar__cta">
          Explore
        </router-link>
        <button class="navbar__hamburger" @click="toggleMobileMenu" :class="{ 'navbar__hamburger--open': mobileMenuOpen }">
          <span></span>
          <span></span>
          <span></span>
        </button>
      </div>
    </div>

    <!-- Mobile Menu -->
    <div class="navbar__mobile" :class="{ 'navbar__mobile--open': mobileMenuOpen }">
      <div class="navbar__mobile-links">
        <a
          v-for="link in navLinks"
          :key="link.path"
          @click="navigateTo(link.path)"
          class="navbar__mobile-link"
          :class="{ 'navbar__mobile-link--active': route.path === link.path }"
        >
          {{ link.name }}
        </a>
      </div>
    </div>
  </nav>
</template>

<style scoped>
.navbar {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: var(--z-nav);
  height: 72px;
  transition: var(--transition-base);
}

.navbar--scrolled {
  background: var(--glass-bg);
  backdrop-filter: blur(20px);
  -webkit-backdrop-filter: blur(20px);
  border-bottom: 1px solid var(--glass-border);
}

.navbar__container {
  display: flex;
  align-items: center;
  justify-content: space-between;
  height: 100%;
}

.navbar__logo {
  display: flex;
  align-items: center;
  gap: var(--space-sm);
  font-family: 'Poppins', sans-serif;
  font-size: 20px;
  font-weight: 500;
  color: var(--white);
}

.navbar__logo-icon {
  width: 24px;
  height: 24px;
  color: var(--gold);
}

.navbar__links {
  display: flex;
  align-items: center;
  gap: var(--space-xl);
}

.navbar__link {
  font-family: 'Inter', sans-serif;
  font-size: 14px;
  font-weight: 500;
  letter-spacing: 0.05em;
  text-transform: uppercase;
  color: var(--gray);
  transition: var(--transition-base);
  position: relative;
  padding-bottom: 4px;
}

.navbar__link:hover {
  color: var(--white);
}

.navbar__link--active {
  color: var(--gold);
}

.navbar__link--active::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 2px;
  background: var(--gold);
  transform: scaleX(1);
}

.navbar__link::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 2px;
  background: var(--gold);
  transform: scaleX(0);
  transform-origin: left;
  transition: transform 0.3s ease;
}

.navbar__link:hover::after {
  transform: scaleX(1);
}

.navbar__actions {
  display: flex;
  align-items: center;
  gap: var(--space-md);
}

.navbar__cta {
  padding: 10px 24px;
  font-size: 12px;
}

.navbar__hamburger {
  display: none;
  flex-direction: column;
  gap: 5px;
  padding: var(--space-sm);
  background: none;
  border: none;
  cursor: pointer;
}

.navbar__hamburger span {
  display: block;
  width: 24px;
  height: 2px;
  background: var(--white);
  transition: var(--transition-base);
}

.navbar__hamburger--open span:nth-child(1) {
  transform: rotate(45deg) translate(5px, 5px);
}

.navbar__hamburger--open span:nth-child(2) {
  opacity: 0;
}

.navbar__hamburger--open span:nth-child(3) {
  transform: rotate(-45deg) translate(5px, -5px);
}

/* Mobile Menu */
.navbar__mobile {
  position: fixed;
  top: 72px;
  left: 0;
  right: 0;
  bottom: 0;
  background: var(--green-deep);
  transform: translateX(100%);
  transition: transform 0.4s ease;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  z-index: 99;
}

.navbar__mobile--open {
  transform: translateX(0);
}

.navbar__mobile-links {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: var(--space-2xl);
}

.navbar__mobile-link {
  font-family: 'Poppins', sans-serif;
  font-size: 32px;
  font-weight: 500;
  color: var(--gray);
  cursor: pointer;
  transition: var(--transition-base);
}

.navbar__mobile-link:hover {
  color: var(--white);
}

.navbar__mobile-link--active {
  color: var(--gold);
}

@media (max-width: 1023px) {
  .navbar__links {
    display: none;
  }

  .navbar__hamburger {
    display: flex;
  }

  .navbar__cta {
    display: none;
  }
}

@media (max-width: 767px) {
  .navbar {
    height: 64px;
  }

  .navbar__mobile {
    top: 64px;
  }

  .navbar__mobile-link {
    font-size: 24px;
  }
}
</style>

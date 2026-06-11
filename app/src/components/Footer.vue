<script setup>
import { RouterLink } from 'vue-router';

const quickLinks = [
  { name: 'Home', path: '/' },
  { name: 'Explore', path: '/explore' },
  { name: 'Gallery', path: '/gallery' },
  { name: 'Trails', path: '/trails' },
  { name: 'Contact', path: '/contact' }
];

const socialLinks = [
  { name: 'Instagram', icon: 'instagram' },
  { name: 'Twitter', icon: 'twitter' },
  { name: 'YouTube', icon: 'youtube' },
  { name: 'Facebook', icon: 'facebook' }
];
</script>

<template>
  <footer class="footer">
    <div class="footer__main">
      <div class="footer__container container">
        <!-- Column 1: Logo & Tagline -->
        <div class="footer__col">
          <router-link to="/" class="footer__logo">
            <svg class="footer__logo-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
              <path d="M8 16L12 8L16 16M12 12V20M4 20H20" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>
            <span class="footer__logo-text">Gede Pangrango</span>
          </router-link>
          <p class="footer__tagline">
            Experience nature at its finest. A premium eco-tourism destination in the heart of West Java.
          </p>
        </div>

        <!-- Column 2: Quick Links -->
        <div class="footer__col">
          <h4 class="footer__heading">Quick Links</h4>
          <ul class="footer__links">
            <li v-for="link in quickLinks" :key="link.path">
              <router-link :to="link.path" class="footer__link link-underline">{{ link.name }}</router-link>
            </li>
          </ul>
        </div>

        <!-- Column 3: Contact Info -->
        <div class="footer__col">
          <h4 class="footer__heading">Contact</h4>
          <ul class="footer__info">
            <li class="footer__info-item">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" width="18" height="18">
                <path d="M17.657 16.657L13.414 20.9a1.998 1.998 0 01-2.827 0l-4.244-4.243a8 8 0 1111.314 0z" stroke-linecap="round" stroke-linejoin="round"/>
                <path d="M15 11a3 3 0 11-6 0 3 3 0 016 0z" stroke-linecap="round" stroke-linejoin="round"/>
              </svg>
              Cibodas, West Java, Indonesia
            </li>
            <li class="footer__info-item">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" width="18" height="18">
                <path d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z" stroke-linecap="round" stroke-linejoin="round"/>
              </svg>
              Open daily, 24 hours
            </li>
            <li class="footer__info-item">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" width="18" height="18">
                <path d="M3 5a2 2 0 012-2h3.28a1 1 0 01.948.684l1.498 4.493a1 1 0 01-.502 1.21l-2.257 1.13a11.042 11.042 0 005.516 5.516l1.13-2.257a1 1 0 011.21-.502l4.493 1.498a1 1 0 01.684.949V19a2 2 0 01-2 2h-1C9.716 21 3 14.284 3 6V5z" stroke-linecap="round" stroke-linejoin="round"/>
              </svg>
              +62 838 7214 7483
            </li>
          </ul>
          <!-- Social Icons -->
          <div class="footer__social">
            <a v-for="social in socialLinks" :key="social.name" href="#" class="footer__social-link" @click.prevent>
              <svg v-if="social.icon === 'instagram'" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" width="20" height="20">
                <rect x="2" y="2" width="20" height="20" rx="5" stroke-linecap="round" stroke-linejoin="round"/>
                <circle cx="12" cy="12" r="5" stroke-linecap="round" stroke-linejoin="round"/>
                <circle cx="17.5" cy="6.5" r="1.5" fill="currentColor" stroke="none"/>
              </svg>
              <svg v-else-if="social.icon === 'twitter'" viewBox="0 0 24 24" fill="currentColor" width="20" height="20">
                <path d="M18.244 2.25h3.308l-7.227 8.26 8.502 11.24H16.17l-5.214-6.817L4.99 21.75H1.68l7.73-8.835L1.254 2.25H8.08l4.713 6.231zm-1.161 17.52h1.833L7.084 4.126H5.117z"/>
              </svg>
              <svg v-else-if="social.icon === 'youtube'" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" width="20" height="20">
                <path d="M22.54 6.42a2.78 2.78 0 00-1.94-2C18.88 4 12 4 12 4s-6.88 0-8.6.46a2.78 2.78 0 00-1.94 2A29 29 0 001 12a29 29 0 00.46 5.58 2.78 2.78 0 001.94 2C5.12 20 12 20 12 20s6.88 0 8.6-.46a2.78 2.78 0 001.94-2A29 29 0 0023 12a29 29 0 00-.46-5.58z" stroke-linecap="round" stroke-linejoin="round"/>
                <polygon points="9.75 15.02 15.5 12 9.75 8.98 9.75 15.02" fill="currentColor" stroke="none"/>
              </svg>
              <svg v-else-if="social.icon === 'facebook'" viewBox="0 0 24 24" fill="currentColor" width="20" height="20">
                <path d="M24 12.073c0-6.627-5.373-12-12-12s-12 5.373-12 12c0 5.99 4.388 10.954 10.125 11.854v-8.385H7.078v-3.47h3.047V9.43c0-3.007 1.792-4.669 4.533-4.669 1.312 0 2.686.235 2.686.235v2.953H15.83c-1.491 0-1.956.925-1.956 1.874v2.25h3.328l-.532 3.47h-2.796v8.385C19.612 23.027 24 18.062 24 12.073z"/>
              </svg>
            </a>
          </div>
        </div>
      </div>
    </div>

    <!-- Bottom Bar -->
    <div class="footer__bottom">
      <div class="container">
        <p class="footer__copyright">
          &copy; {{ new Date().getFullYear() }} Gede Pangrango. All rights reserved. Designed for nature lovers.
        </p>
      </div>
    </div>
  </footer>
</template>

<style scoped>
.footer {
  background: var(--green-deep);
  border-top: 1px solid var(--green-forest);
}

.footer__main {
  padding: var(--space-4xl) 0;
}

.footer__container {
  display: grid;
  grid-template-columns: 2fr 1fr 1.5fr;
  gap: var(--space-4xl);
}

.footer__logo {
  display: flex;
  align-items: center;
  gap: var(--space-sm);
  font-family: 'Poppins', sans-serif;
  font-size: 24px;
  font-weight: 500;
  color: var(--white);
  margin-bottom: var(--space-lg);
}

.footer__logo-icon {
  width: 28px;
  height: 28px;
  color: var(--gold);
}

.footer__tagline {
  font-size: 16px;
  line-height: 1.7;
  color: var(--gray);
  max-width: 320px;
}

.footer__heading {
  font-family: 'Poppins', sans-serif;
  font-size: 18px;
  font-weight: 500;
  color: var(--white);
  margin-bottom: var(--space-lg);
}

.footer__links {
  display: flex;
  flex-direction: column;
  gap: var(--space-md);
}

.footer__link {
  font-size: 14px;
  color: var(--gray);
  transition: var(--transition-base);
}

.footer__link:hover {
  color: var(--gold);
}

.footer__info {
  display: flex;
  flex-direction: column;
  gap: var(--space-md);
  margin-bottom: var(--space-lg);
}

.footer__info-item {
  display: flex;
  align-items: flex-start;
  gap: var(--space-sm);
  font-size: 14px;
  color: var(--gray);
}

.footer__info-item svg {
  flex-shrink: 0;
  margin-top: 2px;
  color: var(--gold);
}

.footer__social {
  display: flex;
  gap: var(--space-md);
}

.footer__social-link {
  color: var(--gray);
  transition: var(--transition-base);
}

.footer__social-link:hover {
  color: var(--gold);
}

.footer__bottom {
  border-top: 1px solid var(--green-forest);
  padding: var(--space-lg) 0;
  text-align: center;
}

.footer__copyright {
  font-size: 14px;
  color: var(--gray);
}

@media (max-width: 1023px) {
  .footer__container {
    grid-template-columns: 1fr 1fr;
    gap: var(--space-3xl);
  }

  .footer__col:first-child {
    grid-column: 1 / -1;
  }
}

@media (max-width: 767px) {
  .footer__container {
    grid-template-columns: 1fr;
    gap: var(--space-2xl);
  }

  .footer__main {
    padding: var(--space-3xl) 0;
  }
}
</style>

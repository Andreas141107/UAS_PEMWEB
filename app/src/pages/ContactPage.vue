<script setup>
import { ref, onMounted } from 'vue';
import SectionTitle from '../components/SectionTitle.vue';
import { faqs } from '../data/wisataData.js';

const form = ref({
  name: '',
  email: '',
  message: ''
});

const errors = ref({});
const submitted = ref(false);
const isLoading = ref(false);
const sendError = ref('');

const openFaq = ref(null);

const validateForm = () => {
  const newErrors = {};

  if (!form.value.name || form.value.name.trim().length < 2) {
    newErrors.name = 'Name is required (min 2 characters)';
  }

  if (!form.value.email || !form.value.email.includes('@')) {
    newErrors.email = 'Valid email is required';
  }

  if (!form.value.message || form.value.message.trim().length < 10) {
    newErrors.message = 'Message is required (min 10 characters)';
  }

  errors.value = newErrors;
  return Object.keys(newErrors).length === 0;
};

const handleSubmit = async () => {
  if (!validateForm()) return;

  isLoading.value = true;
  sendError.value = '';

  try {
    // Kirim via EmailJS
    // Ganti YOUR_SERVICE_ID, YOUR_TEMPLATE_ID, YOUR_PUBLIC_KEY
    // dengan credential dari https://dashboard.emailjs.com
    const response = await fetch('https://api.emailjs.com/api/v1.0/email/send', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        service_id: 'service_b62zt7i',
        template_id: 'template_bjv6z1k',
        user_id: 'KOvgw9sxwV1XctmcL',
        template_params: {
          from_name: form.value.name,
          from_email: form.value.email,
          message: form.value.message,
        }
      })
    });

    if (response.ok) {
      submitted.value = true;
      form.value = { name: '', email: '', message: '' };
      setTimeout(() => {
        submitted.value = false;
      }, 5000);
    } else {
      throw new Error('Failed to send');
    }
  } catch (err) {
    sendError.value = 'Failed to send message. Please try again or contact us directly via email.';
  } finally {
    isLoading.value = false;
  }
};

const toggleFaq = (id) => {
  openFaq.value = openFaq.value === id ? null : id;
};

onMounted(() => {
  const observer = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          entry.target.classList.add('is-visible');
          observer.unobserve(entry.target);
        }
      });
    },
    { threshold: 0.1 }
  );

  document.querySelectorAll('.scroll-reveal').forEach((el) => {
    observer.observe(el);
  });
});
</script>

<template>
  <div class="contact">
    <!-- Page Header -->
    <section class="page-header" style="height: 40vh;">
      <div class="page-header__bg">
        <img src="/images/8_Nature_Mountain_Lake_at_Dawn_Bright.png" alt="Mountain mist" />
      </div>
      <div class="page-header__overlay"></div>
      <div class="page-header__content">
        <p class="font-caption" style="color: var(--gold); margin-bottom: var(--space-md);">CONTACT</p>
        <h1 class="font-display-lg" style="color: var(--white);">Get in Touch</h1>
        <p class="font-body-lg" style="color: var(--gray); margin-top: var(--space-md);">
          We're here to help plan your mountain adventure
        </p>
      </div>
    </section>

    <!-- Contact Grid -->
    <section class="contact-grid section">
      <div class="container">
        <div class="contact-grid__layout">
          <!-- Contact Form -->
          <div class="contact-form scroll-reveal">
            <!-- Success Alert -->
            <div v-if="submitted" class="alert-success">
              <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" style="display: inline; margin-right: 8px;">
                <path d="M22 11.08V12a10 10 0 11-5.93-9.14" stroke-linecap="round" stroke-linejoin="round"/>
                <path d="M22 4L12 14.01l-3-3" stroke-linecap="round" stroke-linejoin="round"/>
              </svg>
              Thank you! Your message has been sent successfully.
            </div>

            <!-- Error Alert -->
            <div v-if="sendError" class="alert-error">
              <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" style="display: inline; margin-right: 8px;">
                <circle cx="12" cy="12" r="10"/>
                <path d="M12 8v4M12 16h.01" stroke-linecap="round" stroke-linejoin="round"/>
              </svg>
              {{ sendError }}
            </div>

            <form @submit.prevent="handleSubmit">
              <div class="form-group">
                <label class="form-label">Name</label>
                <input
                  v-model="form.name"
                  type="text"
                  class="form-input"
                  placeholder="Your name"
                  :class="{ 'form-input--error': errors.name }"
                  :disabled="isLoading"
                />
                <p v-if="errors.name" class="form-error">{{ errors.name }}</p>
              </div>

              <div class="form-group">
                <label class="form-label">Email</label>
                <input
                  v-model="form.email"
                  type="email"
                  class="form-input"
                  placeholder="your@email.com"
                  :class="{ 'form-input--error': errors.email }"
                  :disabled="isLoading"
                />
                <p v-if="errors.email" class="form-error">{{ errors.email }}</p>
              </div>

              <div class="form-group">
                <label class="form-label">Message</label>
                <textarea
                  v-model="form.message"
                  class="form-textarea"
                  placeholder="Tell us about your adventure plans..."
                  :class="{ 'form-textarea--error': errors.message }"
                  :disabled="isLoading"
                ></textarea>
                <p v-if="errors.message" class="form-error">{{ errors.message }}</p>
              </div>

              <button type="submit" class="btn btn-primary btn-full" :disabled="isLoading">
                <span v-if="isLoading" class="btn-loading">
                  <svg class="spinner" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" width="18" height="18">
                    <path d="M12 2v4M12 18v4M4.93 4.93l2.83 2.83M16.24 16.24l2.83 2.83M2 12h4M18 12h4M4.93 19.07l2.83-2.83M16.24 7.76l2.83-2.83" stroke-linecap="round"/>
                  </svg>
                  Sending...
                </span>
                <span v-else>Send Message</span>
              </button>
            </form>
          </div>

          <!-- Info & Map -->
          <div class="contact-info scroll-reveal stagger-2">
            <div class="info-card">
              <div class="info-card__item">
                <div class="info-card__icon">
                  <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" width="24" height="24">
                    <path d="M17.657 16.657L13.414 20.9a1.998 1.998 0 01-2.827 0l-4.244-4.243a8 8 0 1111.314 0z" stroke-linecap="round" stroke-linejoin="round"/>
                    <path d="M15 11a3 3 0 11-6 0 3 3 0 016 0z" stroke-linecap="round" stroke-linejoin="round"/>
                  </svg>
                </div>
                <div class="info-card__text">
                  <p class="info-card__label">Location</p>
                  <p class="info-card__value">Cibodas, West Java, Indonesia</p>
                </div>
              </div>

              <div class="info-card__item">
                <div class="info-card__icon">
                  <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" width="24" height="24">
                    <circle cx="12" cy="12" r="10"/>
                    <path d="M12 6v6l4 2" stroke-linecap="round" stroke-linejoin="round"/>
                  </svg>
                </div>
                <div class="info-card__text">
                  <p class="info-card__label">Operating Hours</p>
                  <p class="info-card__value">Open daily, 24 hours</p>
                </div>
              </div>

              <div class="info-card__item">
                <div class="info-card__icon">
                  <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" width="24" height="24">
                    <path d="M3 5a2 2 0 012-2h3.28a1 1 0 01.948.684l1.498 4.493a1 1 0 01-.502 1.21l-2.257 1.13a11.042 11.042 0 005.516 5.516l1.13-2.257a1 1 0 011.21-.502l4.493 1.498a1 1 0 01.684.949V19a2 2 0 01-2 2h-1C9.716 21 3 14.284 3 6V5z" stroke-linecap="round" stroke-linejoin="round"/>
                  </svg>
                </div>
                <div class="info-card__text">
                  <p class="info-card__label">Phone</p>
                  <p class="info-card__value">+62 838 7214 7483</p>
                </div>
              </div>

              <div class="info-card__item">
                <div class="info-card__icon">
                  <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" width="24" height="24">
                    <path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z" stroke-linecap="round" stroke-linejoin="round"/>
                    <path d="M22 6l-10 7L2 6" stroke-linecap="round" stroke-linejoin="round"/>
                  </svg>
                </div>
                <div class="info-card__text">
                  <p class="info-card__label">Email</p>
                  <p class="info-card__value">info@gedepangrango.go.id</p>
                </div>
              </div>
            </div>

            <!-- Google Maps Embed - Gunung Gede Pangrango -->
            <div class="map-container">
              <iframe
                src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d3962.1!2d106.9833!3d-6.7833!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x2e69b6b7e7e7e7e7%3A0x7e7e7e7e7e7e7e7e!2sTaman%20Nasional%20Gunung%20Gede%20Pangrango!5e0!3m2!1sen!2sid!4v1699999999999!5m2!1sen!2sid&q=Taman+Nasional+Gunung+Gede+Pangrango,+Cibodas,+Cianjur,+West+Java"
                width="100%"
                height="280"
                style="border: 0; border-radius: var(--radius-lg);"
                allowfullscreen=""
                loading="lazy"
                referrerpolicy="no-referrer-when-downgrade"
                title="Lokasi Taman Nasional Gunung Gede Pangrango"
              ></iframe>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- FAQ Section -->
    <section class="faq-section section">
      <div class="container">
        <SectionTitle
          eyebrow="FAQ"
          title="Frequently Asked Questions"
          description="Find answers to common questions about visiting Gunung Gede Pangrango"
        />
        <div class="faq-list scroll-reveal">
          <div
            v-for="(faq, index) in faqs"
            :key="faq.id"
            class="faq-item"
            :class="{ 'faq-item--open': openFaq === faq.id }"
            :style="`transition-delay: ${index * 0.08}s`"
          >
            <div class="faq-item__header" @click="toggleFaq(faq.id)">
              <p class="faq-item__question">{{ faq.question }}</p>
              <button class="faq-item__chevron" :class="{ 'faq-item__chevron--open': openFaq === faq.id }">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" width="20" height="20">
                  <path d="M19 9l-7 7-7-7" stroke-linecap="round" stroke-linejoin="round"/>
                </svg>
              </button>
            </div>
            <div
              class="faq-item__body"
              :class="{ 'faq-item__body--open': openFaq === faq.id }"
            >
              <div class="faq-item__answer">
                <p>{{ faq.answer }}</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<style scoped>
.section {
  padding: var(--space-4xl) 0;
}

.contact-grid {
  background: var(--black);
}

.contact-grid__layout {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: var(--space-4xl);
}

/* Contact Form */
.contact-form {
  background: var(--green-forest);
  border-radius: var(--radius-lg);
  padding: var(--space-2xl);
}

.form-group {
  margin-bottom: var(--space-lg);
}

.form-input--error,
.form-textarea--error {
  border-color: var(--error) !important;
}

/* Alert */
.alert-success {
  background: rgba(34, 197, 94, 0.1);
  border: 1px solid rgba(34, 197, 94, 0.3);
  color: #86efac;
  border-radius: var(--radius-sm);
  padding: var(--space-md) var(--space-lg);
  margin-bottom: var(--space-lg);
  font-size: 14px;
  display: flex;
  align-items: center;
}

.alert-error {
  background: rgba(239, 68, 68, 0.1);
  border: 1px solid rgba(239, 68, 68, 0.3);
  color: #fca5a5;
  border-radius: var(--radius-sm);
  padding: var(--space-md) var(--space-lg);
  margin-bottom: var(--space-lg);
  font-size: 14px;
  display: flex;
  align-items: center;
  gap: 8px;
}

/* Loading spinner */
.btn-loading {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: var(--space-sm);
}

.spinner {
  animation: spin 1s linear infinite;
}

@keyframes spin {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}

.btn:disabled {
  opacity: 0.6;
  cursor: not-allowed;
}

/* Contact Info */
.info-card {
  background: var(--green-forest);
  border-radius: var(--radius-lg);
  padding: var(--space-2xl);
}

.info-card__item {
  display: flex;
  align-items: flex-start;
  gap: var(--space-md);
  margin-bottom: var(--space-lg);
}

.info-card__item:last-child {
  margin-bottom: 0;
}

.info-card__icon {
  width: 48px;
  height: 48px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: rgba(201, 162, 39, 0.1);
  border-radius: var(--radius-sm);
  color: var(--gold);
  flex-shrink: 0;
}

.info-card__text {
  flex: 1;
}

.info-card__label {
  font-family: 'Inter', sans-serif;
  font-size: 12px;
  font-weight: 500;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--gray);
  margin-bottom: 4px;
}

.info-card__value {
  font-size: 16px;
  color: var(--white);
}

/* Google Maps */
.map-container {
  margin-top: var(--space-xl);
  border-radius: var(--radius-lg);
  overflow: hidden;
  border: 1px solid var(--green-forest);
}

/* FAQ Section */
.faq-section {
  background: var(--green-deep);
}

.faq-list {
  max-width: 800px;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  gap: var(--space-md);
}

.faq-item {
  background: var(--green-forest);
  border-radius: var(--radius-md);
  overflow: hidden;
  transition: var(--transition-base);
}

.faq-item:hover {
  border-color: var(--green-primary);
}

.faq-item__header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: var(--space-lg);
  cursor: pointer;
}

.faq-item__question {
  font-size: 16px;
  font-weight: 500;
  color: var(--white);
  padding-right: var(--space-md);
}

.faq-item__chevron {
  width: 32px;
  height: 32px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--gray);
  background: none;
  border: none;
  cursor: pointer;
  transition: transform 0.3s ease;
  flex-shrink: 0;
}

.faq-item__chevron--open {
  transform: rotate(180deg);
}

.faq-item__body {
  max-height: 0;
  overflow: hidden;
  transition: max-height 0.4s ease;
}

.faq-item__body--open {
  max-height: 300px;
}

.faq-item__answer {
  padding: 0 var(--space-lg) var(--space-lg);
}

.faq-item__answer p {
  font-size: 16px;
  line-height: 1.7;
  color: var(--gray);
}

/* Scroll reveal */
.scroll-reveal {
  opacity: 0;
  transform: translateY(30px);
  transition: opacity 0.6s ease, transform 0.6s ease;
}

.scroll-reveal.is-visible {
  opacity: 1;
  transform: translateY(0);
}

@media (max-width: 1023px) {
  .contact-grid__layout {
    grid-template-columns: 1fr;
    gap: var(--space-3xl);
  }

  .contact-info {
    order: -1;
  }
}

@media (max-width: 767px) {
  .section {
    padding: var(--space-3xl) 0;
  }

  .contact-form,
  .info-card {
    padding: var(--space-xl);
  }

  .faq-item__question {
    font-size: 14px;
  }
}
</style>


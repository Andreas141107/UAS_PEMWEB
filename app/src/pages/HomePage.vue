<script setup>
import { ref, onMounted } from 'vue';
import { useRouter } from 'vue-router';
import HeroSection from '../components/HeroSection.vue';
import SectionTitle from '../components/SectionTitle.vue';
import ActivityCard from '../components/ActivityCard.vue';
import InfoBadge from '../components/InfoBadge.vue';
import { destination, highlights, activities, testimonials } from '../data/wisataData.js';

const router = useRouter();

// Get featured activities (first 3)
const featuredActivities = activities.slice(0, 3);

// Scroll reveal observer
const observeElements = () => {
  const observer = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          entry.target.classList.add('is-visible');
          observer.unobserve(entry.target);
        }
      });
    },
    { threshold: 0.1, rootMargin: '0px 0px -80px 0px' }
  );

  document.querySelectorAll('.scroll-reveal').forEach((el) => {
    observer.observe(el);
  });
};

onMounted(() => {
  observeElements();
});
</script>

<template>
  <div class="home">
    <!-- Hero Section -->
    <HeroSection
      eyebrow="TAMAN NASIONAL"
      :title="`Explore The Unspoiled Beauty of <span>Gunung Gede Pangrango</span>`"
      :subtitle="destination.tagline"
      image="/images/6_Home_Gede_Pangrango_Mountain_Official.png"
      primaryButtonText="Explore Nature"
      primaryButtonLink="/explore"
      secondaryButtonText="View Gallery"
      secondaryButtonLink="/gallery"
    />

    <!-- Quick Info Bar -->
    <section class="quick-info">
      <div class="container">
        <div class="quick-info__grid">
          <div class="quick-info__item scroll-reveal">
            <p class="quick-info__label">Elevation</p>
            <p class="quick-info__value">2,958 Meters</p>
          </div>
          <div class="quick-info__item scroll-reveal stagger-1">
            <p class="quick-info__label">Status</p>
            <p class="quick-info__value">Active Volcano</p>
          </div>
          <div class="quick-info__item scroll-reveal stagger-2">
            <p class="quick-info__label">Location</p>
            <p class="quick-info__value">West Java, Indonesia</p>
          </div>
          <div class="quick-info__item scroll-reveal stagger-3">
            <p class="quick-info__label">Best Time</p>
            <p class="quick-info__value">April — October</p>
          </div>
        </div>
      </div>
    </section>

    <!-- Highlights Section -->
    <section class="highlights section">
      <div class="container">
        <SectionTitle
          eyebrow="WHY VISIT"
          title="Experience Nature at Its Finest"
          description="Discover the wonders that await you at Gunung Gede Pangrango National Park"
        />
        <div class="highlights__grid grid-4 scroll-reveal">
          <div v-for="highlight in highlights" :key="highlight.id" class="highlights__item">
            <div class="highlights__icon">
              <svg v-if="highlight.icon === 'sunrise'" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" width="32" height="32">
                <path d="M12 2v4M4.93 4.93l2.83 2.83M20.07 4.93l-2.83 2.83M6 12h12M4 16h16M2 20h20" stroke-linecap="round" stroke-linejoin="round"/>
              </svg>
              <svg v-else-if="highlight.icon === 'trail'" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" width="32" height="32">
                <path d="M8 16l-4-4 4-4M16 8l4 4-4 4M10 20l4-16" stroke-linecap="round" stroke-linejoin="round"/>
              </svg>
              <svg v-else-if="highlight.icon === 'forest'" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" width="32" height="32">
                <path d="M12 2L2 22h20L12 2zm0 4l6 12H6l6-12z" stroke-linecap="round" stroke-linejoin="round"/>
              </svg>
              <svg v-else-if="highlight.icon === 'waterfall'" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" width="32" height="32">
                <path d="M12 2v20M8 6c0-2 2-3 4-3s4 1 4 3M6 10c0-2 2-3 4-3s4 1 4 3M10 14c0-2 2-3 4-3s4 1 4 3" stroke-linecap="round" stroke-linejoin="round"/>
              </svg>
            </div>
            <h3 class="highlights__title">{{ highlight.title }}</h3>
            <p class="highlights__description">{{ highlight.description }}</p>
          </div>
        </div>
      </div>
    </section>

    <!-- Featured Landscape Section -->
    <section class="featured section">
      <div class="container">
        <div class="featured__grid grid-2">
          <div class="featured__image scroll-reveal">
            <img src="/images/9_hiking_trail_with_trees_covered_with.png" alt="Gunung Gede Forest Trail" />
          </div>
          <div class="featured__content scroll-reveal stagger-2">
            <p class="featured__eyebrow">ABOUT THE PARK</p>
            <h2 class="featured__title">A Sanctuary of Natural Wonders</h2>
            <p class="featured__text">{{ destination.description }}</p>
            <p class="featured__text">{{ destination.longDescription }}</p>
            <div class="featured__badges">
              <InfoBadge text="Mountain Escape" />
              <InfoBadge text="Forest Adventure" />
              <InfoBadge text="Sunrise View" />
              <InfoBadge text="Nature Healing" />
            </div>
            <button class="btn btn-primary" @click="router.push('/trails')">
              Explore Trails
            </button>
          </div>
        </div>
      </div>
    </section>

    <!-- Featured Activities Section -->
    <section class="activities section">
      <div class="container">
        <SectionTitle
          eyebrow="FEATURED"
          title="Curated Experiences"
          description="Handpicked activities for the ultimate mountain adventure"
        />
        <div class="activities__grid grid-3">
          <ActivityCard
            v-for="activity in featuredActivities"
            :key="activity.id"
            :activity="activity"
            class="scroll-reveal"
          />
        </div>
        <div class="activities__cta scroll-reveal">
          <button class="btn btn-secondary" @click="router.push('/explore')">
            View All Activities
          </button>
        </div>
      </div>
    </section>

    <!-- Immersive Preview Section -->
    <section class="immersive">
      <div class="immersive__split">
        <div class="immersive__half">
          <img src="/images/3_Mountain_Lake_Reflection_Dawn_Alpine.png" alt="Mountain Lake" />
        </div>
        <div class="immersive__half">
          <img src="/images/8_Nature_Mountain_Lake_at_Dawn_Bright.png" alt="Mountain Mist" />
        </div>
      </div>
      <div class="immersive__overlay"></div>
      <div class="immersive__content scroll-reveal">
        <h2 class="immersive__title">A Landscape Unlike Any Other</h2>
        <p class="immersive__subtitle">From misty peaks to serene lakes, every corner of Gunung Gede Pangrango tells a story of nature's grandeur waiting to be discovered.</p>
        <button class="btn btn-secondary" @click="router.push('/gallery')">
          View Full Gallery
        </button>
      </div>
    </section>

    <!-- Testimonials Section -->
    <section class="testimonials section">
      <div class="container">
        <SectionTitle
          eyebrow="TESTIMONIALS"
          title="What Visitors Say"
          description="Real stories from those who have experienced the magic of Gunung Gede Pangrango"
        />
        <div class="testimonials__grid grid-3">
          <div v-for="testimonial in testimonials" :key="testimonial.id" class="testimonial-card scroll-reveal">
            <span class="testimonial-card__quote-mark">"</span>
            <p class="testimonial-card__quote">{{ testimonial.quote }}</p>
            <div class="testimonial-card__author">
              <img :src="testimonial.avatar" :alt="testimonial.name" class="testimonial-card__avatar" />
              <div class="testimonial-card__author-info">
                <p class="testimonial-card__name">{{ testimonial.name }}</p>
                <p class="testimonial-card__role">{{ testimonial.role }}</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- CTA Section -->
    <section class="cta-section">
      <div class="container">
        <div class="cta-section__content scroll-reveal">
          <h2 class="cta-section__title">Your Journey Begins Here</h2>
          <p class="cta-section__subtitle">Plan your visit to Gunung Gede Pangrango and experience the beauty of pristine nature, challenging trails, and unforgettable moments.</p>
          <div class="cta-section__buttons">
            <button class="btn btn-primary" @click="router.push('/contact')">
              Contact Us
            </button>
            <button class="btn btn-ghost" @click="router.push('/trails')">
              View Trails
            </button>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<style scoped>
/* Section spacing */
.section {
  padding: var(--space-5xl) 0;
}

/* Scroll reveal */
.scroll-reveal {
  opacity: 0;
  transform: translateY(40px);
  transition: opacity 0.8s ease, transform 0.8s ease;
}

.scroll-reveal.is-visible {
  opacity: 1;
  transform: translateY(0);
}

/* Highlights */
.highlights {
  background: var(--black);
}

.grid-4 {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: var(--space-xl);
}

.highlights__item {
  text-align: center;
  padding: var(--space-xl);
  background: rgba(23, 63, 42, 0.3);
  border: 1px solid rgba(47, 107, 79, 0.2);
  border-radius: var(--radius-lg);
  transition: var(--transition-slow);
}

.highlights__item:hover {
  border-color: var(--green-primary);
  transform: translateY(-4px);
  box-shadow: var(--shadow-float);
}

.highlights__icon {
  width: 64px;
  height: 64px;
  margin: 0 auto var(--space-lg);
  display: flex;
  align-items: center;
  justify-content: center;
  background: rgba(201, 162, 39, 0.1);
  border-radius: 50%;
  color: var(--gold);
}

.highlights__title {
  font-family: 'Poppins', sans-serif;
  font-size: 20px;
  font-weight: 500;
  color: var(--white);
  margin-bottom: var(--space-sm);
}

.highlights__description {
  font-size: 14px;
  line-height: 1.6;
  color: var(--gray);
}

/* Featured */
.featured {
  background: var(--green-deep);
}

.featured__image img {
  width: 100%;
  border-radius: var(--radius-lg);
  aspect-ratio: 16/10;
  object-fit: cover;
}

.featured__content {
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding-left: var(--space-xl);
}

.featured__eyebrow {
  font-family: 'Inter', sans-serif;
  font-size: 12px;
  font-weight: 500;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--gold);
  margin-bottom: var(--space-md);
}

.featured__title {
  font-family: 'Poppins', sans-serif;
  font-size: clamp(28px, 3.5vw, 42px);
  font-weight: 500;
  line-height: 1.1;
  color: var(--white);
  margin-bottom: var(--space-lg);
}

.featured__text {
  font-size: 16px;
  line-height: 1.7;
  color: var(--gray);
  margin-bottom: var(--space-md);
}

.featured__badges {
  display: flex;
  flex-wrap: wrap;
  gap: var(--space-sm);
  margin-bottom: var(--space-xl);
}

/* Activities */
.activities {
  background: var(--black);
}

.activities__cta {
  text-align: center;
  margin-top: var(--space-3xl);
}

/* Immersive */
.immersive {
  position: relative;
  height: 60vh;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}

.immersive__split {
  position: absolute;
  inset: 0;
  display: grid;
  grid-template-columns: 1fr 1fr;
}

.immersive__half {
  overflow: hidden;
}

.immersive__half img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.immersive__overlay {
  position: absolute;
  inset: 0;
  background: rgba(0, 0, 0, 0.55);
}

.immersive__content {
  position: relative;
  z-index: 2;
  text-align: center;
  max-width: 640px;
  padding: 0 var(--container-pad);
}

.immersive__title {
  font-family: 'Poppins', sans-serif;
  font-size: clamp(28px, 3.5vw, 48px);
  font-weight: 500;
  color: var(--white);
  margin-bottom: var(--space-md);
}

.immersive__subtitle {
  font-size: 18px;
  line-height: 1.8;
  color: var(--gray);
  margin-bottom: var(--space-xl);
}

/* Testimonials */
.testimonials {
  background: var(--black);
}

.testimonial-card {
  position: relative;
  padding-top: var(--space-xl);
  border-top: 2px solid var(--gold);
  transition: var(--transition-slow);
}

.testimonial-card:hover {
  transform: translateY(-4px);
}

.testimonial-card__quote-mark {
  font-family: 'Cormorant Garamond', serif;
  font-size: 80px;
  line-height: 1;
  color: var(--gold);
  opacity: 0.3;
  position: absolute;
  top: var(--space-lg);
  left: 0;
}

.testimonial-card__quote {
  font-size: 16px;
  line-height: 1.7;
  font-style: italic;
  color: var(--white);
  margin-bottom: var(--space-xl);
  padding-top: var(--space-2xl);
}

.testimonial-card__author {
  display: flex;
  align-items: center;
  gap: var(--space-md);
}

.testimonial-card__avatar {
  width: 48px;
  height: 48px;
  border-radius: 50%;
  object-fit: cover;
}

.testimonial-card__name {
  font-size: 16px;
  font-weight: 500;
  color: var(--white);
}

.testimonial-card__role {
  font-size: 14px;
  color: var(--gray);
}

/* CTA Section */
.cta-section {
  padding: var(--space-5xl) 0;
  background: var(--green-forest);
}

.cta-section__content {
  text-align: center;
  max-width: 640px;
  margin: 0 auto;
}

.cta-section__title {
  font-family: 'Poppins', sans-serif;
  font-size: clamp(28px, 3.5vw, 48px);
  font-weight: 500;
  color: var(--white);
  margin-bottom: var(--space-md);
}

.cta-section__subtitle {
  font-size: 18px;
  line-height: 1.8;
  color: var(--gray);
  margin-bottom: var(--space-xl);
}

.cta-section__buttons {
  display: flex;
  gap: var(--space-md);
  justify-content: center;
  flex-wrap: wrap;
}

/* Responsive */
@media (max-width: 1023px) {
  .grid-4 {
    grid-template-columns: repeat(2, 1fr);
  }

  .featured__content {
    padding-left: 0;
    padding-top: var(--space-2xl);
  }
}

@media (max-width: 767px) {
  .grid-4 {
    grid-template-columns: 1fr;
  }

  .section {
    padding: var(--space-3xl) 0;
  }

  .immersive {
    height: 50vh;
  }

  .immersive__split {
    grid-template-columns: 1fr;
  }

  .immersive__half:last-child {
    display: none;
  }

  .cta-section__buttons {
    flex-direction: column;
    align-items: center;
  }

  .cta-section__buttons .btn {
    width: 100%;
    max-width: 280px;
  }
}
</style>

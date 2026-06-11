<script setup>
import { ref, computed, watch, nextTick, onMounted } from 'vue';
import { useRouter } from 'vue-router';
import TrailCard from '../components/TrailCard.vue';
import SectionTitle from '../components/SectionTitle.vue';
import { trails } from '../data/wisataData.js';

const router = useRouter();
const activeDifficulty = ref('All');
const difficulties = ['All', 'Easy', 'Medium', 'Hard'];

const filteredTrails = computed(() => {
  if (activeDifficulty.value === 'All') {
    return trails;
  }
  return trails.filter((t) => t.difficulty === activeDifficulty.value);
});

const resetFilter = () => {
  activeDifficulty.value = 'All';
};

const viewpoints = [
  {
    name: 'Puncak Gunung Gede',
    description: 'Summit crater view with panoramic 360-degree vistas',
    bestTime: 'Sunrise',
    image: '/images/6_Home_Gede_Pangrango_Mountain_Official.png'
  },
  {
    name: 'Alun-Alun Suryakencana',
    description: 'Edelweiss meadow with stunning volcanic backdrop',
    bestTime: 'Morning',
    image: '/images/5_2_734_Edelweiss_Mountain_Stock_Photos.png'
  },
  {
    name: 'Telaga Biru',
    description: 'Blue volcanic lake with surreal turquoise waters',
    bestTime: 'Midday',
    image: '/images/2_Mountain_Lake_Reflection_Dawn_Alpine.png'
  }
];

// Jalankan observer setelah DOM update
const runRevealObserver = () => {
  nextTick(() => {
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

    // Reset is-visible dulu, lalu observe ulang semua item
    document.querySelectorAll('.scroll-reveal').forEach((el) => {
      el.classList.remove('is-visible');
      observer.observe(el);
    });
  });
};

// Setiap kali filteredTrails berubah, re-trigger animasi reveal
watch(filteredTrails, () => {
  runRevealObserver();
});

onMounted(() => {
  runRevealObserver();
});
</script>

<template>
  <div class="trails">
    <!-- Page Header -->
    <section class="page-header">
      <div class="page-header__bg">
        <img src="/images/9_hiking_trail_with_trees_covered_with.png" alt="Trail map" />
      </div>
      <div class="page-header__overlay"></div>
      <div class="page-header__content">
        <p class="font-caption" style="color: var(--gold); margin-bottom: var(--space-md);">TRAILS</p>
        <h1 class="font-display-lg" style="color: var(--white);">Choose Your Path</h1>
        <p class="font-body-lg" style="color: var(--gray); margin-top: var(--space-md);">
          From gentle walks to challenging summit treks
        </p>
      </div>
    </section>

    <!-- Difficulty Filter -->
    <section class="filter-bar">
      <div class="container">
        <div class="filter-bar__content">
          <div class="filter-bar__pills">
            <button
              v-for="diff in difficulties"
              :key="diff"
              class="filter-pill"
              :class="{ active: activeDifficulty === diff }"
              @click="activeDifficulty = diff"
            >
              {{ diff }}
            </button>
          </div>
          <button class="filter-bar__reset link-underline" @click="resetFilter" v-if="activeDifficulty !== 'All'">
            Reset
          </button>
        </div>
      </div>
    </section>

    <!-- Trail List -->
    <section class="trail-list section">
      <div class="container">
        <div v-if="filteredTrails.length > 0" class="trail-list__content">
          <TrailCard
            v-for="(trail, index) in filteredTrails"
            :key="trail.id"
            :trail="trail"
            class="scroll-reveal"
            :style="`transition-delay: ${index * 0.1}s`"
          />
        </div>

        <!-- Empty State -->
        <div v-else class="empty-state">
          <div class="empty-state__icon">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1" width="64" height="64">
              <path d="M8 16l-4-4 4-4M16 8l4 4-4 4M10 20l4-16" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>
          </div>
          <h3 class="empty-state__title">No trails found</h3>
          <p class="empty-state__desc">Try adjusting your difficulty filter</p>
          <button class="btn btn-secondary" @click="resetFilter" style="margin-top: var(--space-lg);">
            Reset Filter
          </button>
        </div>
      </div>
    </section>

    <!-- Recommendations -->
    <section class="recommendations section">
      <div class="container">
        <SectionTitle
          eyebrow="DON'T MISS"
          title="Recommended Viewpoints"
          description="The best spots to capture the beauty of Gunung Gede Pangrango"
        />
        <div class="recommendations__grid">
          <div
            v-for="(vp, index) in viewpoints"
            :key="index"
            class="viewpoint-card scroll-reveal"
            :style="`transition-delay: ${index * 0.12}s`"
          >
            <div class="viewpoint-card__image">
              <img :src="vp.image" :alt="vp.name" />
            </div>
            <h3 class="viewpoint-card__name">{{ vp.name }}</h3>
            <p class="viewpoint-card__desc">{{ vp.description }}</p>
            <span class="viewpoint-card__time">Best time: {{ vp.bestTime }}</span>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<style scoped>
.filter-bar {
  padding: var(--space-lg) 0;
  background: var(--green-deep);
  border-bottom: 1px solid var(--green-forest);
  position: sticky;
  top: 0;
  z-index: 50;
}

.filter-bar__content {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: var(--space-lg);
  flex-wrap: wrap;
}

.filter-bar__pills {
  display: flex;
  gap: var(--space-sm);
  flex-wrap: wrap;
  justify-content: center;
}

.filter-bar__reset {
  font-size: 14px;
  color: var(--gold);
  background: none;
  border: none;
  cursor: pointer;
  font-family: 'Inter', sans-serif;
}

.trail-list {
  padding: var(--space-3xl) 0;
  background: var(--black);
  min-height: 40vh;
}

.trail-list__content {
  display: flex;
  flex-direction: column;
  gap: var(--space-lg);
  max-width: var(--container-max);
  margin: 0 auto;
}

.scroll-reveal {
  opacity: 0;
  transform: translateY(30px);
  transition: opacity 0.6s ease, transform 0.6s ease;
}

.scroll-reveal.is-visible {
  opacity: 1;
  transform: translateY(0);
}

/* Recommendations */
.recommendations {
  padding: var(--space-3xl) 0;
  background: var(--green-deep);
}

.recommendations__grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: var(--space-xl);
}

.viewpoint-card {
  transition: var(--transition-slow);
}

.viewpoint-card:hover {
  transform: translateY(-4px);
}

.viewpoint-card__image {
  border-radius: var(--radius-lg);
  overflow: hidden;
  margin-bottom: var(--space-md);
}

.viewpoint-card__image img {
  width: 100%;
  aspect-ratio: 16/10;
  object-fit: cover;
  transition: transform 0.5s ease;
}

.viewpoint-card:hover .viewpoint-card__image img {
  transform: scale(1.05);
}

.viewpoint-card__name {
  font-family: 'Poppins', sans-serif;
  font-size: 20px;
  font-weight: 500;
  color: var(--white);
  margin-bottom: var(--space-xs);
}

.viewpoint-card__desc {
  font-size: 14px;
  line-height: 1.6;
  color: var(--gray);
  margin-bottom: var(--space-sm);
}

.viewpoint-card__time {
  font-family: 'Inter', sans-serif;
  font-size: 11px;
  font-weight: 500;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--gold);
}

@media (max-width: 1023px) {
  .recommendations__grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 767px) {
  .recommendations__grid {
    grid-template-columns: 1fr;
  }

  .filter-bar__pills {
    overflow-x: auto;
    flex-wrap: nowrap;
    padding-bottom: var(--space-sm);
    -webkit-overflow-scrolling: touch;
  }
}
</style>
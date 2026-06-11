<script setup>
import { ref, computed, watch, nextTick, onMounted } from 'vue';
import ActivityCard from '../components/ActivityCard.vue';
import { activities } from '../data/wisataData.js';

const searchQuery = ref('');
const activeCategory = ref('All');

const categories = ['All', 'Hiking', 'Camping', 'Nature', 'Photography'];

const filteredActivities = computed(() => {
  let result = activities;

  // Step 1: filter by active category
  if (activeCategory.value !== 'All') {
    result = result.filter((a) => a.category === activeCategory.value);
  }

  // Step 2: filter by search query (dalam hasil kategori yang sudah difilter)
  if (searchQuery.value.trim()) {
    const query = searchQuery.value.toLowerCase();
    result = result.filter(
      (a) =>
        a.title.toLowerCase().includes(query) ||
        a.description.toLowerCase().includes(query)
    );
  }

  return result;
});

const resetFilters = () => {
  searchQuery.value = '';
  activeCategory.value = 'All';
};

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

// Setiap kali filteredActivities berubah, re-trigger animasi reveal
watch(filteredActivities, () => {
  runRevealObserver();
});

onMounted(() => {
  runRevealObserver();
});
</script>

<template>
  <div class="explore">
    <!-- Page Header -->
    <section class="page-header">
      <div class="page-header__bg">
        <img src="/images/4_Exploring_Hoh_Rain_Forest_Trails.png" alt="Forest path" />
      </div>
      <div class="page-header__overlay"></div>
      <div class="page-header__content">
        <p class="font-caption" style="color: var(--gold); margin-bottom: var(--space-md);">EXPLORE</p>
        <h1 class="font-display-lg" style="color: var(--white);">Curated Experiences</h1>
        <p class="font-body-lg" style="color: var(--gray); margin-top: var(--space-md);">
          Discover activities for every kind of adventurer
        </p>
      </div>
    </section>

    <!-- Search & Filter Bar -->
    <section class="filter-bar">
      <div class="container">
        <div class="filter-bar__content">
          <div class="filter-bar__search">
            <svg class="filter-bar__search-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <circle cx="11" cy="11" r="8"/>
              <path d="M21 21l-4.35-4.35" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>
            <input
              v-model="searchQuery"
              type="text"
              placeholder="Search activities..."
              class="filter-bar__input"
            />
          </div>
          <div class="filter-bar__pills">
            <button
              v-for="cat in categories"
              :key="cat"
              class="filter-pill"
              :class="{ active: activeCategory === cat }"
              @click="activeCategory = cat"
            >
              {{ cat }}
            </button>
          </div>
          <button class="filter-bar__reset link-underline" @click="resetFilters" v-if="searchQuery || activeCategory !== 'All'">
            Reset
          </button>
        </div>
      </div>
    </section>

    <!-- Activity Grid -->
    <section class="activity-grid section">
      <div class="container">
        <div v-if="filteredActivities.length > 0" class="activity-grid__list">
          <ActivityCard
            v-for="(activity, index) in filteredActivities"
            :key="activity.id"
            :activity="activity"
            class="scroll-reveal"
            :style="`transition-delay: ${index * 0.08}s`"
          />
        </div>

        <!-- Empty State -->
        <div v-else class="empty-state">
          <div class="empty-state__icon">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1" width="64" height="64">
              <path d="M8 16L12 8L16 16M12 12V20M4 20H20" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>
          </div>
          <h3 class="empty-state__title">No activities found</h3>
          <p class="empty-state__desc">Try adjusting your search or filters</p>
          <button class="btn btn-secondary" @click="resetFilters" style="margin-top: var(--space-lg);">
            Reset Filters
          </button>
        </div>
      </div>
    </section>
  </div>
</template>

<style scoped>
.filter-bar {
  padding: var(--space-lg) 0;
  background: var(--green-deep);
  position: sticky;
  top: 0;
  z-index: 50;
  border-bottom: 1px solid var(--green-forest);
}

.filter-bar__content {
  display: flex;
  align-items: center;
  gap: var(--space-lg);
  flex-wrap: wrap;
}

.filter-bar__search {
  position: relative;
  flex: 1;
  min-width: 250px;
}

.filter-bar__search-icon {
  position: absolute;
  left: var(--space-md);
  top: 50%;
  transform: translateY(-50%);
  width: 18px;
  height: 18px;
  color: var(--gray);
  pointer-events: none;
}

.filter-bar__input {
  width: 100%;
  height: 48px;
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid var(--green-forest);
  border-radius: 9999px;
  padding: 0 var(--space-lg) 0 calc(var(--space-lg) + 24px);
  color: var(--white);
  font-family: 'Inter', sans-serif;
  font-size: 16px;
  transition: var(--transition-base);
}

.filter-bar__input::placeholder {
  color: var(--gray);
}

.filter-bar__input:focus {
  border-color: var(--green-primary);
  background: rgba(255, 255, 255, 0.08);
}

.filter-bar__pills {
  display: flex;
  gap: var(--space-sm);
  flex-wrap: wrap;
}

.filter-bar__reset {
  font-size: 14px;
  color: var(--gold);
  background: none;
  border: none;
  cursor: pointer;
  font-family: 'Inter', sans-serif;
}

.activity-grid {
  padding: var(--space-3xl) 0;
  background: var(--black);
  min-height: 50vh;
}

.activity-grid__list {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: var(--space-xl);
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

@media (max-width: 1023px) {
  .activity-grid__list {
    grid-template-columns: repeat(2, 1fr);
  }

  .filter-bar__content {
    flex-direction: column;
    align-items: stretch;
  }
}

@media (max-width: 767px) {
  .activity-grid__list {
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
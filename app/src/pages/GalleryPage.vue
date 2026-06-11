<script setup>
import { ref, computed, watch, nextTick, onMounted, onUnmounted } from 'vue';
import GalleryCard from '../components/GalleryCard.vue';
import { gallery } from '../data/wisataData.js';

const searchQuery = ref('');
const activeCategory = ref('All');
const selectedImage = ref(null);

const categories = ['All', 'Sunrise', 'Hiking', 'Forest', 'Camping', 'Waterfall', 'Landscape'];

// ✅ FIX: Filter kategori dijalankan dulu, baru search.
// Search hanya mengecek title — bukan category — supaya tidak
// bisa "melewati" filter kategori yang sedang aktif.
const filteredGallery = computed(() => {
  let result = gallery;

  // Step 1: filter by active category
  if (activeCategory.value !== 'All') {
    result = result.filter((g) => g.category === activeCategory.value);
  }

  // Step 2: filter by search query (title only, within already-filtered result)
  if (searchQuery.value.trim()) {
    const query = searchQuery.value.toLowerCase();
    result = result.filter((g) =>
      g.title.toLowerCase().includes(query)
    );
  }

  return result;
});

const resetFilters = () => {
  searchQuery.value = '';
  activeCategory.value = 'All';
};

// Lightbox
const openModal = (item) => {
  selectedImage.value = item;
  document.body.style.overflow = 'hidden';
};

const closeModal = () => {
  selectedImage.value = null;
  document.body.style.overflow = '';
};

const navigateModal = (direction) => {
  if (!selectedImage.value) return;
  const currentIndex = filteredGallery.value.findIndex((g) => g.id === selectedImage.value.id);

  // ✅ FIX: Kalau item yang sedang dibuka tidak ada di filteredGallery
  // (misal karena filter berubah saat modal terbuka), fallback ke index 0
  if (currentIndex === -1) {
    selectedImage.value = filteredGallery.value[0] ?? null;
    return;
  }

  const newIndex =
    direction === 'next'
      ? (currentIndex + 1) % filteredGallery.value.length
      : (currentIndex - 1 + filteredGallery.value.length) % filteredGallery.value.length;

  selectedImage.value = filteredGallery.value[newIndex];
};

// Keyboard navigation
const handleKeydown = (e) => {
  if (!selectedImage.value) return;
  if (e.key === 'Escape') closeModal();
  if (e.key === 'ArrowLeft') navigateModal('prev');
  if (e.key === 'ArrowRight') navigateModal('next');
};

// Jalankan observer setelah DOM update — dipanggil saat mount & setiap filter berubah
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
      { threshold: 0.05 }
    );

    // Reset is-visible dulu, lalu observe ulang semua item
    document.querySelectorAll('.masonry-reveal').forEach((el) => {
      el.classList.remove('is-visible');
      observer.observe(el);
    });
  });
};

// Setiap kali filteredGallery berubah, re-trigger animasi reveal
watch(filteredGallery, () => {
  runRevealObserver();
});

onMounted(() => {
  window.addEventListener('keydown', handleKeydown);
  runRevealObserver();
});

onUnmounted(() => {
  window.removeEventListener('keydown', handleKeydown);
});

// ✅ Computed sudah di-unwrap otomatis di template, tidak ada masalah di sini
const getImageIndex = computed(() => {
  if (!selectedImage.value) return 0;
  const idx = filteredGallery.value.findIndex((g) => g.id === selectedImage.value.id);
  // Kalau tidak ketemu (edge case), kembalikan 0 supaya counter tidak menampilkan -1 + 1 = 0 yang membingungkan
  return idx === -1 ? 0 : idx + 1;
});
</script>

<template>
  <div class="gallery">
    <!-- Page Header -->
    <section class="page-header">
      <div class="page-header__bg">
        <img src="/images/6_Home_Gede_Pangrango_Mountain_Official.png" alt="Sunset mountain" />
      </div>
      <div class="page-header__overlay"></div>
      <div class="page-header__content">
        <p class="font-caption" style="color: var(--gold); margin-bottom: var(--space-md);">GALLERY</p>
        <h1 class="font-display-lg" style="color: var(--white);">Through the Lens</h1>
        <p class="font-body-lg" style="color: var(--gray); margin-top: var(--space-md);">
          Moments captured in the heart of the mountains
        </p>
      </div>
    </section>

    <!-- Category Filter -->
    <section class="filter-section">
      <div class="container">
        <div class="filter-section__content">
          <div class="filter-section__search">
            <svg class="filter-section__search-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <circle cx="11" cy="11" r="8"/>
              <path d="M21 21l-4.35-4.35" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>
            <input
              v-model="searchQuery"
              type="text"
              placeholder="Search gallery..."
              class="filter-section__input"
            />
          </div>
          <div class="filter-section__pills">
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
          <button
            class="filter-section__reset link-underline"
            @click="resetFilters"
            v-if="searchQuery || activeCategory !== 'All'"
          >
            Reset
          </button>
        </div>
      </div>
    </section>

    <!-- Masonry Grid -->
    <section class="gallery-grid section">
      <div class="container">
        <div v-if="filteredGallery.length > 0" class="masonry-grid">
          <div
            v-for="(item, index) in filteredGallery"
            :key="item.id"
            class="masonry-item masonry-reveal"
            :style="`transition-delay: ${index * 0.05}s`"
          >
            <GalleryCard :item="item" @open-modal="openModal" />
          </div>
        </div>

        <!-- Empty State -->
        <div v-else class="empty-state">
          <div class="empty-state__icon">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1" width="64" height="64">
              <rect x="3" y="3" width="18" height="18" rx="2" stroke-linecap="round" stroke-linejoin="round"/>
              <circle cx="8.5" cy="8.5" r="1.5" fill="currentColor" stroke="none"/>
              <path d="M21 15l-5-5L5 21" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>
          </div>
          <h3 class="empty-state__title">No photos found</h3>
          <p class="empty-state__desc">Try adjusting your search or filters</p>
          <button class="btn btn-secondary" @click="resetFilters" style="margin-top: var(--space-lg);">
            Reset Filters
          </button>
        </div>
      </div>
    </section>

    <!-- Lightbox Modal -->
    <div v-if="selectedImage" class="modal-overlay" @click="closeModal">
      <div class="modal-content" @click.stop>
        <button class="modal-close" @click="closeModal">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" width="32" height="32">
            <path d="M18 6L6 18M6 6l12 12" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
        </button>
        <button class="modal-nav modal-nav--prev" @click="navigateModal('prev')">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" width="24" height="24">
            <path d="M15 18l-6-6 6-6" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
        </button>
        <img :src="selectedImage.image" :alt="selectedImage.title" class="modal-image" />
        <button class="modal-nav modal-nav--next" @click="navigateModal('next')">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" width="24" height="24">
            <path d="M9 18l6-6-6-6" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
        </button>
        <span class="modal-counter">{{ getImageIndex }} / {{ filteredGallery.length }}</span>
      </div>
    </div>
  </div>
</template>

<style scoped>
.filter-section {
  padding: var(--space-lg) 0;
  background: var(--green-deep);
  border-bottom: 1px solid var(--green-forest);
}

.filter-section__content {
  display: flex;
  align-items: center;
  gap: var(--space-lg);
  flex-wrap: wrap;
}

.filter-section__search {
  position: relative;
  flex: 1;
  min-width: 250px;
}

.filter-section__search-icon {
  position: absolute;
  left: var(--space-md);
  top: 50%;
  transform: translateY(-50%);
  width: 18px;
  height: 18px;
  color: var(--gray);
  pointer-events: none;
}

.filter-section__input {
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

.filter-section__input::placeholder {
  color: var(--gray);
}

.filter-section__input:focus {
  border-color: var(--green-primary);
}

.filter-section__pills {
  display: flex;
  gap: var(--space-sm);
  flex-wrap: wrap;
}

.filter-section__reset {
  font-size: 14px;
  color: var(--gold);
  background: none;
  border: none;
  cursor: pointer;
  font-family: 'Inter', sans-serif;
}

.gallery-grid {
  padding: var(--space-3xl) 0;
  background: var(--black);
  min-height: 50vh;
}

.masonry-grid {
  columns: 3;
  column-gap: var(--space-md);
}

.masonry-item {
  break-inside: avoid;
  margin-bottom: var(--space-md);
}

.masonry-reveal {
  opacity: 0;
  transform: translateY(20px);
  transition: opacity 0.5s ease, transform 0.5s ease;
}

.masonry-reveal.is-visible {
  opacity: 1;
  transform: translateY(0);
}

.modal-overlay {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.95);
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  z-index: 1000;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: var(--space-xl);
  animation: fadeIn 0.3s ease;
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

.modal-content {
  position: relative;
  max-width: 90vw;
  max-height: 85vh;
  animation: scaleIn 0.3s ease;
}

@keyframes scaleIn {
  from {
    opacity: 0;
    transform: scale(0.9);
  }
  to {
    opacity: 1;
    transform: scale(1);
  }
}

.modal-image {
  max-width: 100%;
  max-height: 85vh;
  object-fit: contain;
  border-radius: var(--radius-md);
}

.modal-close {
  position: absolute;
  top: -48px;
  right: 0;
  width: 40px;
  height: 40px;
  color: var(--white);
  cursor: pointer;
  transition: var(--transition-base);
  background: rgba(255, 255, 255, 0.1);
  border: none;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal-close:hover {
  color: var(--gold);
  background: rgba(255, 255, 255, 0.2);
}

.modal-nav {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  width: 48px;
  height: 48px;
  color: var(--white);
  cursor: pointer;
  transition: var(--transition-base);
  background: rgba(255, 255, 255, 0.1);
  border: none;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal-nav:hover {
  color: var(--gold);
  background: rgba(255, 255, 255, 0.2);
}

.modal-nav--prev {
  left: -64px;
}

.modal-nav--next {
  right: -64px;
}

.modal-counter {
  position: absolute;
  bottom: -40px;
  left: 50%;
  transform: translateX(-50%);
  font-family: 'Inter', sans-serif;
  font-size: 12px;
  font-weight: 500;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--gray);
}

@media (max-width: 1023px) {
  .masonry-grid {
    columns: 2;
  }

  .modal-nav--prev {
    left: var(--space-sm);
  }

  .modal-nav--next {
    right: var(--space-sm);
  }
}

@media (max-width: 767px) {
  .masonry-grid {
    columns: 1;
  }

  .modal-nav {
    width: 40px;
    height: 40px;
  }

  .filter-section__content {
    flex-direction: column;
    align-items: stretch;
  }

  .filter-section__pills {
    overflow-x: auto;
    flex-wrap: nowrap;
    padding-bottom: var(--space-sm);
  }
}
</style>
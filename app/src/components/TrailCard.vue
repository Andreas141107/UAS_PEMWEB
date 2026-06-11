<script setup>
import { ref } from 'vue';

const props = defineProps({
  trail: {
    type: Object,
    required: true
  }
});

const isExpanded = ref(false);

const toggleAccordion = () => {
  isExpanded.value = !isExpanded.value;
};

const getDifficultyClass = (difficulty) => {
  switch (difficulty.toLowerCase()) {
    case 'easy':
      return 'badge-easy';
    case 'medium':
      return 'badge-medium';
    case 'hard':
      return 'badge-hard';
    default:
      return 'badge-easy';
  }
};

const handleViewMap = () => {
  alert(`Peta untuk ${props.trail.title} akan ditampilkan di sini.`);
};
</script>

<template>
  <div class="trail-card">
    <!-- Header / Collapsed State -->
    <div class="trail-card__header" @click="toggleAccordion">
      <img :src="trail.image" :alt="trail.title" class="trail-card__thumb" />
      <div class="trail-card__info">
        <h3 class="trail-card__title">{{ trail.title }}</h3>
        <p class="trail-card__location">
          <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path d="M17.657 16.657L13.414 20.9a1.998 1.998 0 01-2.827 0l-4.244-4.243a8 8 0 1111.314 0z" stroke-linecap="round" stroke-linejoin="round"/>
            <path d="M15 11a3 3 0 11-6 0 3 3 0 016 0z" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
          {{ trail.location }}
        </p>
        <div class="trail-card__badges">
          <span class="trail-card__badge">
            <svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <circle cx="12" cy="12" r="10"/>
              <path d="M12 6v6l4 2" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>
            {{ trail.duration }}
          </span>
          <span class="trail-card__badge">
            <svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <path d="M13 7h8m0 0v8m0-8l-8 8-4-4-6 6" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>
            {{ trail.distance }}
          </span>
          <span :class="getDifficultyClass(trail.difficulty)">
            {{ trail.difficulty }}
          </span>
        </div>
      </div>
      <button class="trail-card__chevron" :class="{ 'trail-card__chevron--open': isExpanded }">
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M19 9l-7 7-7-7" stroke-linecap="round" stroke-linejoin="round"/>
        </svg>
      </button>
    </div>

    <!-- Expanded Content -->
    <div class="trail-card__body" :class="{ 'trail-card__body--open': isExpanded }" :style="{ height: isExpanded ? 'auto' : '0' }">
      <div class="trail-card__content">
        <p class="trail-card__description">{{ trail.description }}</p>
        <div class="trail-card__details">
          <div class="trail-card__highlights">
            <h4>Highlights</h4>
            <ul>
              <li v-for="(highlight, index) in trail.highlights" :key="index">
                <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <path d="M5 13l4 4L19 7" stroke-linecap="round" stroke-linejoin="round"/>
                </svg>
                {{ highlight }}
              </li>
            </ul>
          </div>
          <div class="trail-card__tips">
            <h4>Tips</h4>
            <p>{{ trail.tips }}</p>
          </div>
        </div>
        <button class="btn btn-secondary trail-card__map-btn" @click="handleViewMap">
          View on Map
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.trail-card {
  background: var(--green-forest);
  border-radius: var(--radius-lg);
  overflow: hidden;
  border: 1px solid rgba(47, 107, 79, 0.2);
  transition: var(--transition-slow);
}

.trail-card:hover {
  border-color: var(--green-primary);
  box-shadow: var(--shadow-float);
}

.trail-card__header {
  display: flex;
  align-items: center;
  gap: var(--space-lg);
  padding: var(--space-xl);
  cursor: pointer;
}

.trail-card__thumb {
  width: 120px;
  height: 90px;
  object-fit: cover;
  border-radius: var(--radius-sm);
  flex-shrink: 0;
}

.trail-card__info {
  flex: 1;
  min-width: 0;
}

.trail-card__title {
  font-family: 'Poppins', sans-serif;
  font-size: 20px;
  font-weight: 500;
  color: var(--white);
  margin-bottom: var(--space-xs);
}

.trail-card__location {
  display: flex;
  align-items: center;
  gap: 6px;
  font-size: 14px;
  color: var(--gray);
  margin-bottom: var(--space-sm);
}

.trail-card__location svg {
  flex-shrink: 0;
  color: var(--gold);
}

.trail-card__badges {
  display: flex;
  align-items: center;
  gap: var(--space-sm);
  flex-wrap: wrap;
}

.trail-card__badge {
  display: flex;
  align-items: center;
  gap: 4px;
  padding: 4px 10px;
  background: rgba(0, 0, 0, 0.2);
  border-radius: var(--radius-full);
  font-size: 11px;
  font-weight: 500;
  letter-spacing: 0.05em;
  text-transform: uppercase;
  color: var(--gray);
}

.trail-card__badge svg {
  color: var(--green-primary);
}

.trail-card__chevron {
  width: 40px;
  height: 40px;
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

.trail-card__chevron--open {
  transform: rotate(180deg);
}

.trail-card__body {
  overflow: hidden;
  transition: height 0.4s ease;
}

.trail-card__content {
  padding: 0 var(--space-xl) var(--space-xl);
}

.trail-card__description {
  font-size: 16px;
  line-height: 1.7;
  color: var(--gray);
  margin-bottom: var(--space-lg);
}

.trail-card__details {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: var(--space-lg);
  margin-bottom: var(--space-lg);
}

.trail-card__highlights h4,
.trail-card__tips h4 {
  font-family: 'Poppins', sans-serif;
  font-size: 14px;
  font-weight: 500;
  color: var(--gold);
  margin-bottom: var(--space-sm);
  letter-spacing: 0.05em;
  text-transform: uppercase;
}

.trail-card__highlights ul {
  display: flex;
  flex-direction: column;
  gap: var(--space-sm);
}

.trail-card__highlights li {
  display: flex;
  align-items: flex-start;
  gap: var(--space-sm);
  font-size: 14px;
  color: var(--white);
}

.trail-card__highlights li svg {
  flex-shrink: 0;
  color: var(--green-primary);
  margin-top: 2px;
}

.trail-card__tips {
  background: rgba(201, 162, 39, 0.1);
  padding: var(--space-md);
  border-radius: var(--radius-sm);
}

.trail-card__tips p {
  font-size: 14px;
  line-height: 1.6;
  color: var(--gray);
}

.trail-card__map-btn {
  padding: 10px 24px;
  font-size: 12px;
}

@media (max-width: 767px) {
  .trail-card__header {
    flex-direction: column;
    align-items: flex-start;
  }

  .trail-card__thumb {
    width: 100%;
    height: 160px;
  }

  .trail-card__chevron {
    align-self: flex-end;
    margin-top: -40px;
    background: rgba(0, 0, 0, 0.5);
    border-radius: 50%;
    backdrop-filter: blur(4px);
  }

  .trail-card__details {
    grid-template-columns: 1fr;
  }
}
</style>

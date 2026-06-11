<script setup>
const props = defineProps({
  activity: {
    type: Object,
    required: true
  }
});

const handleViewDetail = () => {
  alert(`Detail untuk aktivitas: ${props.activity.title}\n\n${props.activity.description}\n\nDurasi: ${props.activity.duration}\nTingkat Kesulitan: ${props.activity.difficulty}`);
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
</script>

<template>
  <div class="activity-card card">
    <div class="activity-card__image">
      <img :src="activity.image" :alt="activity.title" />
      <div class="activity-card__category">{{ activity.category }}</div>
    </div>
    <div class="activity-card__content">
      <h3 class="activity-card__title">{{ activity.title }}</h3>
      <p class="activity-card__description">{{ activity.description }}</p>
      <div class="activity-card__meta">
        <span class="activity-card__duration">
          <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <circle cx="12" cy="12" r="10"/>
            <path d="M12 6v6l4 2" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
          {{ activity.duration }}
        </span>
        <span :class="getDifficultyClass(activity.difficulty)">
          {{ activity.difficulty }}
        </span>
      </div>
      <button class="activity-card__button" @click="handleViewDetail">
        <span>View Detail</span>
        <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M5 12h14M12 5l7 7-7 7" stroke-linecap="round" stroke-linejoin="round"/>
        </svg>
      </button>
    </div>
  </div>
</template>

<style scoped>
.activity-card {
  display: flex;
  flex-direction: column;
  height: 100%;
}

.activity-card__image {
  position: relative;
  aspect-ratio: 16/10;
  overflow: hidden;
}

.activity-card__image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.5s ease;
}

.activity-card:hover .activity-card__image img {
  transform: scale(1.05);
}

.activity-card__category {
  position: absolute;
  top: var(--space-md);
  left: var(--space-md);
  padding: 4px 12px;
  background: rgba(0, 0, 0, 0.6);
  backdrop-filter: blur(4px);
  border-radius: var(--radius-full);
  font-family: 'Inter', sans-serif;
  font-size: 11px;
  font-weight: 500;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--gold);
}

.activity-card__content {
  padding: var(--space-lg);
  display: flex;
  flex-direction: column;
  flex: 1;
}

.activity-card__title {
  font-family: 'Poppins', sans-serif;
  font-size: 20px;
  font-weight: 500;
  line-height: 1.3;
  color: var(--white);
  margin-bottom: var(--space-sm);
}

.activity-card__description {
  font-size: 14px;
  line-height: 1.6;
  color: var(--gray);
  margin-bottom: var(--space-md);
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
  overflow: hidden;
  flex: 1;
}

.activity-card__meta {
  display: flex;
  align-items: center;
  gap: var(--space-md);
  margin-bottom: var(--space-md);
  padding-bottom: var(--space-md);
  border-bottom: 1px solid rgba(47, 107, 79, 0.2);
}

.activity-card__duration {
  display: flex;
  align-items: center;
  gap: 6px;
  font-size: 12px;
  font-weight: 500;
  letter-spacing: 0.05em;
  text-transform: uppercase;
  color: var(--gray);
}

.activity-card__duration svg {
  color: var(--green-primary);
}

.activity-card__button {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
  padding: var(--space-sm) 0;
  font-family: 'Inter', sans-serif;
  font-size: 13px;
  font-weight: 500;
  letter-spacing: 0.05em;
  text-transform: uppercase;
  color: var(--white);
  background: none;
  border: none;
  cursor: pointer;
  transition: var(--transition-base);
}

.activity-card__button:hover {
  color: var(--gold);
}

.activity-card__button svg {
  transition: transform 0.3s ease;
}

.activity-card__button:hover svg {
  transform: translateX(4px);
}
</style>

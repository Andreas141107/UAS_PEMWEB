<script setup>
import { onMounted, ref } from 'vue';
import { useRouter } from 'vue-router';

const props = defineProps({
  title: {
    type: String,
    default: ''
  },
  subtitle: {
    type: String,
    default: ''
  },
  image: {
    type: String,
    default: ''
  },
  primaryButtonText: {
    type: String,
    default: 'Explore'
  },
  primaryButtonLink: {
    type: String,
    default: '/explore'
  },
  secondaryButtonText: {
    type: String,
    default: ''
  },
  secondaryButtonLink: {
    type: String,
    default: ''
  },
  eyebrow: {
    type: String,
    default: ''
  }
});

const router = useRouter();
const heroRef = ref(null);
const imageLoaded = ref(false);

onMounted(() => {
  const img = new Image();
  img.src = props.image;
  img.onload = () => {
    imageLoaded.value = true;
  };
});
</script>

<template>
  <section class="hero" ref="heroRef">
    <div class="hero__bg">
      <img :src="image" :alt="title" :class="{ 'hero__bg--loaded': imageLoaded }" />
    </div>
    <div class="hero__overlay"></div>
    <div class="hero__content">
      <p v-if="eyebrow" class="hero__eyebrow">{{ eyebrow }}</p>
      <h1 class="hero__title" v-html="title"></h1>
      <p v-if="subtitle" class="hero__subtitle">{{ subtitle }}</p>
      <div class="hero__buttons">
        <button class="btn btn-primary" @click="router.push(primaryButtonLink)">
          {{ primaryButtonText }}
        </button>
        <button
          v-if="secondaryButtonText"
          class="btn btn-ghost"
          @click="router.push(secondaryButtonLink)"
        >
          {{ secondaryButtonText }}
        </button>
      </div>
    </div>
    <div class="hero__scroll-indicator scroll-indicator">
      <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <path d="M12 5v14M5 12l7 7 7-7" stroke-linecap="round" stroke-linejoin="round"/>
      </svg>
    </div>
  </section>
</template>

<style scoped>
.hero {
  position: relative;
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}

.hero__bg {
  position: absolute;
  inset: 0;
  z-index: 1;
}

.hero__bg img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  opacity: 0;
  transform: scale(1.05);
  transition: opacity 1.5s ease, transform 1.5s ease;
}

.hero__bg--loaded {
  opacity: 1 !important;
  transform: scale(1) !important;
}

.hero__overlay {
  position: absolute;
  inset: 0;
  background: linear-gradient(to bottom, rgba(0, 0, 0, 0.3) 0%, rgba(11, 30, 37, 0.6) 100%);
  z-index: 2;
}

.hero__content {
  position: relative;
  z-index: 3;
  text-align: center;
  max-width: 900px;
  margin: 0 auto;
  padding: 0 var(--container-pad);
  animation: fadeInUp 0.8s ease forwards;
}

.hero__eyebrow {
  font-family: 'Inter', sans-serif;
  font-size: 12px;
  font-weight: 500;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--gold);
  margin-bottom: var(--space-md);
}

.hero__title {
  font-family: 'Poppins', sans-serif;
  font-size: clamp(40px, 7vw, 84px);
  line-height: 1.0;
  font-weight: 400;
  letter-spacing: -0.02em;
  color: var(--white);
  margin-bottom: var(--space-lg);
}

.hero__title :deep(span) {
  color: var(--gold);
}

.hero__subtitle {
  font-family: 'Inter', sans-serif;
  font-size: 18px;
  line-height: 1.8;
  color: var(--gray);
  max-width: 560px;
  margin: 0 auto var(--space-xl);
}

.hero__buttons {
  display: flex;
  gap: var(--space-md);
  justify-content: center;
  flex-wrap: wrap;
}

.hero__scroll-indicator {
  position: absolute;
  bottom: var(--space-2xl);
  left: 50%;
  transform: translateX(-50%);
  z-index: 3;
  color: var(--white);
  opacity: 0.6;
}

@media (max-width: 767px) {
  .hero__title {
    font-size: clamp(32px, 10vw, 48px);
  }

  .hero__subtitle {
    font-size: 16px;
  }

  .hero__buttons {
    flex-direction: column;
    align-items: center;
  }

  .hero__buttons .btn {
    width: 100%;
    max-width: 280px;
  }
}
</style>

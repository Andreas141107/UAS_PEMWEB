<script setup>
const props = defineProps({
  eyebrow: {
    type: String,
    default: ''
  },
  title: {
    type: String,
    required: true
  },
  description: {
    type: String,
    default: ''
  }
});
</script>

<template>
  <div class="section-title">
    <p v-if="eyebrow" class="section-title__eyebrow">{{ eyebrow }}</p>
    <h2 class="section-title__title">{{ title }}</h2>
    <p v-if="description" class="section-title__desc">{{ description }}</p>
  </div>
</template>

<style scoped>
.section-title {
  text-align: center;
  margin-bottom: var(--space-3xl);
}

.section-title__eyebrow {
  font-family: 'Inter', sans-serif;
  font-size: 12px;
  font-weight: 500;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--gold);
  margin-bottom: var(--space-sm);
}

.section-title__title {
  font-family: 'Poppins', sans-serif;
  font-size: clamp(28px, 3.5vw, 48px);
  font-weight: 500;
  line-height: 1.1;
  color: var(--white);
}

.section-title__desc {
  font-family: 'Inter', sans-serif;
  font-size: 16px;
  line-height: 1.7;
  color: var(--gray);
  margin-top: var(--space-md);
  max-width: 560px;
  margin-left: auto;
  margin-right: auto;
}
</style>

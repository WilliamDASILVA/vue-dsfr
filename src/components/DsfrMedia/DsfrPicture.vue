<script>
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'DsfrPicture',
  props: {
    alt: {
      type: String,
      default: '',
    },
    legend: {
      type: String,
      default: '',
    },
    size: {
      type: String,
      default: undefined,
      validator: (val) => ['small', 'medium', 'large', undefined].includes(val),
    },
    src: {
      type: String,
      default: undefined,
    },
    title: {
      type: String,
      default: '',
    },
    ratio: {
      type: String,
      default: '16x9',
    },
  },
})
</script>

<template>
  <figure
    class="fr-content-media"
    role="group"
    :class="{
      'fr-content-media--sm': size == 'small',
      'fr-content-media--lg': size == 'large',
    }"
    :aria-label="legend"
  >
    <div class="fr-content-media__img">
      <!-- @slot Slot par défaut pour insérer une image personnalisée (SVG ou autre). Par défaut, insère l’image donnée par la props src -->
      <slot>
        <img
          v-if="src"
          :src="src"
          class="fr-responsive-img"
          :class="`fr-ratio-${ratio}`"
          :alt="alt"
          :title="title"
          :ratio="ratio"
        >
      </slot>
    </div>
    <figcaption class="fr-content-media__caption">
      {{ legend }}
    </figcaption>
  </figure>
</template>

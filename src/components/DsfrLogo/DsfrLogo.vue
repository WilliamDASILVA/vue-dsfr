
<script>
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'DsfrLogo',
  props: {
    small: Boolean,
    large: Boolean,
    logoText: {
      type: [String, Array],
      default: () => 'Gouvernement',
    },
  },

  computed: {
    text () {
      return this.logoText instanceof Array ? this.logoText.join('<br>') : this.logoText
    },
  },
})
</script>

<template>
  <p
    class="fr-logo"
    :class="{
      'fr-logo--sm': small && !large,
      'fr-logo--lg': large && !small,
    }"
    v-html="text"
  />
</template>

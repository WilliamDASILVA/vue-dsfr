<script>
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'DsfrTabItem',

  props: {
    selected: Boolean,
    panelId: {
      type: String,
      required: true,
    },
    icon: {
      type: String,
      default: undefined,
    },
    tabId: {
      type: String,
      required: true,
    },
  },

  emits: ['click', 'next', 'previous', 'first', 'last'],

  watch: {
    selected (newValue, oldValue) {
      if (newValue) {
        this.$refs.button.focus()
      }
    },
  },
})
</script>

<template>
  <li
    role="presentation"
  >
    <button
      :id="tabId"
      ref="button"
      :data-testid="`test-${tabId}`"
      class="fr-tabs__tab"
      :tabindex="selected ? 0 : -1"
      role="tab"
      :aria-selected="selected"
      :aria-controls="panelId"
      @click="$emit('click', $event)"
      @keydown.right="$emit('next')"
      @keydown.left="$emit('previous')"
      @keydown.down="$emit('next')"
      @keydown.up="$emit('previous')"
      @keydown.home="$emit('first')"
      @keydown.end="$emit('last')"
    >
      <span
        v-if="icon"
        style="margin-left: -0.25rem; margin-right: 0.5rem; font-size: 0.95rem;"
      >
        <VIcon
          :name="icon"
        />
      </span>
      <!-- @slot Slot par défaut pour le contenu de l’onglet. Sera dans `<button class="fr-tabs__tab">` -->
      <slot />
    </button>
  </li>
</template>

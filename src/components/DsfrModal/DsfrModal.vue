<script>
import { defineComponent } from 'vue'

import { FocusTrap } from 'focus-trap-vue'

// import '@gouvfr/dsfr/dist/component/modal/modal.module.js'

import DsfrButtonGroup from '../DsfrButton/DsfrButtonGroup.vue'

export default defineComponent({
  name: 'DsfrModal',

  components: {
    DsfrButtonGroup,
    FocusTrap,
  },

  props: {
    opened: Boolean,
    actions: {
      type: Array,
      default: () => [],
    },
    isAlert: Boolean,
    origin: {
      type: Object,
      default: () => ({ focus () {} }),
    },
    title: {
      type: String,
      required: true,
    },
    icon: {
      type: String,
      default: undefined,
    },
  },

  emits: [
    'close',
  ],

  data () {
    return {
      closeIfEscape: ($event) => {
        if ($event.key === 'Escape' || $event.keyCode === 27) {
          this.close()
        }
      },
    }
  },

  computed: {
    role () {
      return this.isAlert ? 'alertdialog' : 'dialog'
    },
  },

  watch: {
    opened (newValue, oldValue) {
      if (newValue) {
        document.body.classList.add('modal-open')
        setTimeout(() => {
          this.$refs.closeBtn.focus()
        }, 100)
      } else {
        document.body.classList.remove('modal-open')
      }
    },
  },

  mounted () {
    this.startListeningToEscape()
  },

  beforeUnmount () {
    this.stopListeningToEscape()
  },

  methods: {
    startListeningToEscape () {
      document.addEventListener('keydown', this.closeIfEscape)
    },

    stopListeningToEscape () {
      document.removeEventListener('keydown', this.closeIfEscape)
    },

    async close () {
      await this.$nextTick()
      this.origin.focus()
      this.$emit('close')
    },
  },
})
</script>

<template>
  <focus-trap
    v-if="opened"
  >
    <dialog
      id="fr-modal-1"
      aria-labelledby="fr-modal-title-modal-1"
      :role="role"
      class="fr-modal"
      :class="{'fr-modal--opened': opened}"
    >
      <div class="fr-container fr-container--fluid fr-container-md">
        <div class="fr-grid-row fr-grid-row--center">
          <div class="fr-col-12 fr-col-md-8 fr-col-lg-6">
            <div class="fr-modal__body">
              <div class="fr-modal__header">
                <button
                  ref="closeBtn"
                  class="fr-btn fr-btn--close"
                  title="Fermer la fenêtre modale"
                  aria-controls="fr-modal-1"
                  type="button"
                  @click="close()"
                >
                  <span>
                    Fermer
                  </span>
                </button>
              </div>
              <div class="fr-modal__content">
                <h1
                  id="fr-modal-title-modal-1"
                  class="fr-modal__title"
                >
                  <span
                    v-if="icon"
                  >
                    <VIcon
                      :name="icon"
                      scale="2"
                    />
                  </span>
                  {{ title }}
                </h1>
                <!-- @slot Slot par défaut pour le contenu de la liste. Sera dans `<ul class="fr-modal__title">` -->
                <slot />
              </div>
              <div
                v-if="actions && actions.length"
                class="fr-modal__footer"
              >
                <DsfrButtonGroup
                  align="right"
                  :buttons="actions"
                  inline
                  reverse
                />
              </div>
            </div>
          </div>
        </div>
      </div>
    </dialog>
  </focus-trap>
</template>

<style scoped>
.fr-modal {
  color: var(--text-default-grey);
}
</style>

<style>
body.modal-open {
  overflow: hidden;
}
</style>

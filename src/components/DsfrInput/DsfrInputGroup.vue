<script>
import { defineComponent } from 'vue'

import DsfrInput from './DsfrInput.vue'

import { getRandomId } from '../../utils/random-utils.js'

export default defineComponent({
  name: 'DsfrInputGroup',
  components: {
    DsfrInput,
  },
  inheritAttrs: false,

  props: {
    id: {
      type: String,
      default: undefined,
    },
    descriptionId: {
      type: String,
      default () {
        return getRandomId('input', 'message-desc')
      },
    },
    label: {
      type: String,
      default: '',
    },
    hint: {
      type: String,
      default: '',
    },
    labelVisible: Boolean,
    modelValue: {
      type: String,
      default: undefined,
    },
    placeholder: {
      type: String,
      default: 'Placeholder',
    },
    errorMessage: {
      type: String,
      default: undefined,
    },
    validMessage: {
      type: String,
      default: undefined,
    },
  },

  emits: ['update:modelValue'],

  computed: {
    message () {
      return this.errorMessage || this.validMessage
    },
    messageClass () {
      return this.errorMessage ? 'fr-error-text' : 'fr-valid-text'
    },
    messageIcon () {
      return this.errorMessage ? 'ri-alert-line' : 'ri-checkbox-circle-line'
    },
  },
  watch: {
    modelValue (nv, ov) {
      console.log({ nv, ov })
    },
  },

})
</script>

<template>
  <div
    class="fr-input-group"
    :class="{
      'fr-input-group--error': errorMessage,
      'fr-input-group--valid': validMessage,
    }"
  >
    <slot name="before-input" />
    <slot />
    <DsfrInput
      v-if="!$slots.default"
      v-bind="$attrs"
      :is-valid="!!validMessage"
      :is-invalid="!!errorMessage"
      :label="label"
      :hint="hint"
      :description-id="descriptionId"
      :label-visible="labelVisible"
      :model-value="modelValue"
      :placeholder="placeholder"
      @update:model-value="$emit('update:modelValue', $event)"
    />
    <div
      v-if="message"
      class="fr-messages-group"
    >
      <p
        :id="descriptionId"
        :data-testid="descriptionId"
        :class="messageClass"
      >
        <span>{{ message }}</span>
      </p>
    </div>
  </div>
</template>

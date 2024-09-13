<template>
  <div>
    <q-list v-if="option.children && option.children.length > 0" class="cursor-pointer">
      <q-item clickable :dense="dense">
        <q-item-section>
          <q-item-label>{{ getLable(option) }}</q-item-label>
        </q-item-section>
        <q-item-section avatar>
          <q-icon :size="expandIconSize" :name="expandIcon" />
        </q-item-section>
        <q-menu
          :anchor="anchor"
          :self="self"
        >
          <menu-item
            v-for="(child, index) in option.children"
            v-model="selfValue"
            :option="child"
            :key="index"
            :option-value="optionValue"
            :option-label="optionLabel"
            :anchor="anchor"
            :self="self"
            :dense="dense"
            :expand-icon="expandIcon"
            :expand-icon-size="expandIconSize"
            @update:model-value="updateValue"
          />
        </q-menu>
      </q-item>
    </q-list>
    <q-item v-else clickable v-close-popup @click="clickItem(option)">
      <q-item-section>
        <q-item-label>{{ getLable(option) }}</q-item-label>
      </q-item-section>
    </q-item>
  </div>
</template>

<script>
import { defineComponent, ref } from 'vue'

export default defineComponent({
  name: 'MenuItem',
  props: {
    option: {
      type: Object,
      required: true
    },
    dense: {
      type: Boolean,
      default: true
    },
    anchor: {
      type: String,
      default: 'top end'
    },
    self: {
      type: String,
      default: 'top middle'
    },
    expandIcon: {
      type: String,
      default: 'keyboard_arrow_right'
    },
    expandIconSize: {
      type: String,
      default: 'xs'
    },
    optionLabel: {
      type: String,
      default: 'label'
    },
    optionValue: {
      type: String,
      default: 'value'
    },
    modelValue: {
      type: Object,
      default: function () {
        return null
      }
    }
  },
  setup (props, { emit }) {
    const selfValue = ref(props.modelValue ? props.modelValue : '')

    const updateValue = (option) => {
      emit('update:modelValue', option)
    }

    const clickItem = (option) => {
      updateValue(option)
    }

    const getLable = (option) => {
      // eslint-disable-next-line no-prototype-builtins
      return (typeof option === 'object' && option.hasOwnProperty(props.optionLabel)) ? option[props.optionLabel] : ''
    }

    return {
      selfValue,
      getLable,
      clickItem,
      updateValue
    }
  }
})
</script>

<template>
  <q-select
    :options="options"
    :option-value="optionValue"
    :option-label="optionLabel"
    :dense="dense"
    :display-value="selfLabel"
    :clearable="clearable"
    :readonly="readonly"
    :disable="disable"
    outlined
  >
    <template v-slot:option="{ opt }">
      <menu-item
        :option="opt"
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
    </template>
  </q-select>
</template>

<script>
import { defineComponent, ref, onMounted } from 'vue'
import MenuItem from 'src/components/MenuItem.vue'
import { debug } from 'src/utils/index'

export default defineComponent({
  name: 'QCascader',
  components: {
    MenuItem
  },
  props: {
    label: {
      type: String,
      default: '请选择'
    },
    options: {
      type: Array,
      required: true
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
    dense: {
      type: Boolean,
      default: false
    },
    optionLabel: {
      type: String,
      default: 'label'
    },
    optionValue: {
      type: String,
      default: 'value'
    },
    index: {
      type: Number,
      default: 0
    },
    clearable: {
      type: Boolean,
      default: false
    },
    disable: {
      type: Boolean,
      default: false
    },
    readonly: {
      type: Boolean,
      default: false
    },
    modelValue: {
      type: Object,
      default: function () {
        return null
      }
    }
  },
  setup (props, { emit }) {
    const selfLabel = ref(props.label ? props.label : '请选择')

    onMounted(() => {
      debug(props.modelValue, '1111')
    })

    const getLabel = (option) => {
      // eslint-disable-next-line no-prototype-builtins
      return (typeof option === 'object' && option.hasOwnProperty(props.optionLabel)) ? option[props.optionLabel] : ''
    }
    const getValue = (option) => {
      // eslint-disable-next-line no-prototype-builtins
      return (typeof option === 'object' && option.hasOwnProperty(props.optionValue)) ? option[props.optionValue] : ''
    }

    const updateValue = (a) => {
      selfLabel.value = getLabel(a)
      const model = { [props.optionValue]: getValue(a), [props.optionLabel]: selfLabel.value }

      emit('update:modelValue', model, props.index)
    }
    return {
      selfLabel,
      getLabel,
      updateValue
    }
  }
})
</script>

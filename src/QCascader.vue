<template>
  <q-btn-dropdown
    :flat="flat"
    :outline="outline"
    :no-caps="noCaps"
    :label="selfLabel"
  >
    <q-list>
      <div v-for="(option, index) in options" :key="'p-' + index">
        <q-item
          v-if="option.children && option.children.length > 0"
            clickable
            :dense="dense"
          >
          <q-item-section>
            <q-item-label>{{ getLabel(option) }}</q-item-label>
          </q-item-section>
          <q-item-section avatar>
            <q-icon :size="expandIconSize" :name="expandIcon" />
          </q-item-section>
          <q-menu
            :anchor="menuAnchor"
            :self="menuSelf"
          >
            <q-list>
              <div v-for="(child, cIndex) in option.children" :key="'p-c-' + cIndex">
                <q-item
                  v-if="child.children && child.children.length > 0"
                  clickable
                >
                  <q-item-section>
                    <q-item-label>{{ getLabel(child) }}</q-item-label>
                  </q-item-section>
                  <q-item-section avatar>
                    <q-icon :size="expandIconSize" :name="expandIcon" />
                  </q-item-section>
                  <q-menu
                    :anchor="menuAnchor"
                    :self="menuSelf"
                  >
                    <q-list style="min-width: 100px">
                      <div v-for="(aChild, aIndex) in child.children" :key="'p-c-a-' + aIndex">
                        <q-item
                          v-if="aChild.children && aChild.children.length > 0"
                          clickable
                        >
                          <q-item-section>
                            <q-item-label>{{ getLabel(aChild) }}</q-item-label>
                          </q-item-section>
                          <q-item-section avatar>
                            <q-icon :size="expandIconSize" :name="expandIcon" />
                          </q-item-section>
                          <q-menu
                            :anchor="menuAnchor"
                            :self="menuSelf"
                          >
                            <q-list style="min-width: 100px">
                              <div v-for="(bChild, bIndex) in aChild.children" :key="'p-c-a-b-' + bIndex">
                                <q-item clickable v-close-popup @click="updateValue(bChild, aChild, child, option)">
                                  <q-item-section>
                                    <q-item-label>{{ getLabel(bChild) }}</q-item-label>
                                  </q-item-section>
                                </q-item>
                              </div>
                            </q-list>
                          </q-menu>
                        </q-item>
                        <q-item v-else clickable v-close-popup @click="updateValue(aChild, child, option)">
                          <q-item-section>
                            <q-item-label>{{ getLabel(aChild) }}</q-item-label>
                          </q-item-section>
                        </q-item>
                      </div>
                    </q-list>
                  </q-menu>
                </q-item>
                <q-item v-else clickable v-close-popup @click="updateValue(child, option)">
                  <q-item-section>
                    <q-item-label>{{ getLabel(child) }}</q-item-label>
                  </q-item-section>
                </q-item>
              </div>
            </q-list>
          </q-menu>
        </q-item>
      </div>
    </q-list>
  </q-btn-dropdown>
</template>

<script>
import { defineComponent, onMounted, ref, watch } from 'vue'

export default defineComponent({
  name: 'QCascader',
  props: {
    label: {
      type: String,
      default: '请选择'
    },
    dense: {
      type: Boolean,
      default: false
    },
    flat: {
      type: Boolean,
      default: true
    },
    outline: {
      type: Boolean,
      default: true
    },
    menuAnchor: {
      type: String,
      default: 'top end'
    },
    menuSelf: {
      type: String,
      default: 'top middle'
    },
    options: {
      type: Array,
      default: function () {
        return []
      }
    },
    optionLabel: {
      type: String,
      default: 'label'
    },
    optionValue: {
      type: String,
      default: 'value'
    },
    noCaps: {
      type: Boolean,
      default: true
    },
    clearAble: {
      type: Boolean,
      default: false
    },
    index: {
      type: Number,
      default: 0
    },
    expandIcon: {
      type: String,
      default: 'keyboard_arrow_right'
    },
    expandIconSize: {
      type: String,
      default: 'xs'
    },
    modelValue: {
      type: Array,
      default: function () {
        return []
      }
    }
  },
  setup (props, { emit }) {
    const selfLabel = ref(props.label ? props.label : '请选择')

    onMounted(() => {
      const value = props.modelValue
      if (!isEmpty(value)) {
        setSelfLabel(value[0] || '', value[1] || '', value[2] || '', value[3] || '')
      }
    })

    watch(() => props.modelValue, (value) => {
      if (!isEmpty(value)) {
        setSelfLabel(value[0] || '', value[1] || '', value[2] || '', value[3] || '')
      }
    })

    const updateValue = (a, b, c, d) => {
      setSelfLabel(a, b, c, d)
      const items = [a]
      if (b !== undefined && b !== '') {
        items.push(b)
      }

      if (c !== undefined && c !== '') {
        items.push(c)
      }

      if (d !== undefined && d !== '') {
        items.push(d)
      }
      emit('update:modelValue', items, props.index)
    }

    const isNull = (val) => {
      return val === undefined || val === null
    }

    const isEmpty = (obj) => {
      return isNull(obj) || (typeof (obj) === 'string' && obj.trim() === '') || (typeof (obj) === 'object' && Object.keys(obj).length < 1)
    }

    const getLabel = (option) => {
      // eslint-disable-next-line no-prototype-builtins
      return (typeof option === 'object' && option.hasOwnProperty(props.optionLabel)) ? option[props.optionLabel] : ''
    }

    const setSelfLabel = (a, b, c, d) => {
      const label = (!isEmpty(d) ? (getLabel(d) + '/') : '') + (!isEmpty(c) ? (getLabel(c) + '/') : '') +
                        ((!isEmpty(b)) ? (getLabel(b) + '/') : '') + getLabel(a)
      if (label !== '') {
        selfLabel.value = label
      }
    }

    return {
      selfLabel,
      updateValue,
      getLabel
    }
  }
})
</script>

<template>
  <q-btn-dropdown flat outline :no-caps="noCaps" :label="selfLabel">
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
            <q-icon size="xs" :name="expandIcon" />
          </q-item-section>
          <q-menu
            anchor="top end"
            self="top middle"
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
                    <q-icon size="xs" :name="expandIcon" />
                  </q-item-section>
                  <q-menu
                    anchor="top end"
                    self="top middle"
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
                            <q-icon size="xs" :name="expandIcon" />
                          </q-item-section>
                          <q-menu
                            anchor="top end"
                            self="top middle"
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
import { defineComponent, onMounted, onUnmounted, ref } from 'vue'

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
      if (props.modelValue[0] !== undefined && props.modelValue[1] !== undefined) {
        updateValue(props.modelValue[0] || '', props.modelValue[1] || '', props.modelValue[2] || undefined, props.modelValue[3] || '', props.modelValue[4] || '')
      }
    })

    onUnmounted(() => {
    })

    const updateValue = (a, b, c, d) => {
      selfLabel.value = ((d !== undefined && d !== '') ? (getLabel(d) + '/') : '') + ((c !== undefined && c !== '') ? (getLabel(c) + '/') : '') +
                        ((b !== undefined && b !== '') ? (getLabel(b) + '/') : '') + getLabel(a)
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

    const getLabel = (option) => {
      // eslint-disable-next-line no-prototype-builtins
      return (typeof option === 'object' && option.hasOwnProperty(props.optionLabel)) ? option[props.optionLabel] : ''
    }

    return {
      selfLabel,
      updateValue,
      getLabel
    }
  }
})
</script>

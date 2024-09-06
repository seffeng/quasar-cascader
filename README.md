# Vue Quasar Cascader

## 安装

```shell
npm install q-cascader
```

## 示例

```vue
<template>
  <q-cascader v-model="select" :options="options" :index="0" @update:model-value="getValue" />
  <q-cascader v-model="select" :options="options" :index="1" @update:model-value="getValue" />
</template>

<script>
import { defineComponent, ref } from 'vue'
import QCascader from 'q-cascader'

export default defineComponent({
  components: {
    QCascader
  },
  setup () {
    const select = ref([
        {
          value: '1-2',
          label: '1-2--222'
        },
        {
          value: '1',
          label: 'aaa'
        }
    ])
    const options = ref([
      {
        value: '1',
        label: 'aaa',
        children: [
          {
            value: '1-1',
            label: '1-1--1111'
          },
          {
            value: '1-2',
            label: '1-2--222'
          }
        ]
      }, {
        value: '2',
        label: 'bbb',
        children: [
          {
            value: '2-1',
            label: '2-1--1111'
          },
          {
            value: '2-2',
            label: '2-2--222',
            children: [
              {
                value: '2-2-1',
                label: '2-2-1--1111'
              },
              {
                value: '2-2-2',
                label: '2-2-2--222',
                children: [
                  {
                    value: '2-2-2-1',
                    label: '2-2-2-1--1111'
                  }
                ]
              }
            ]
          }
        ]
      }
    ])

    return {
      options,
      getValue (a, index) {
        console.log(a, index)
      }
    }
  }
})
</script>
```

## 参数

```VUE
<q-cascader v-model="select" :options="options" @update:model-value="getValue" />
```

| 参数    | 类型    | 默认               | 说明             |
| ------- | ------- | ------------------ | ---------------------- |
| model-value | array | [] | 回显数据[子Object，..., 父Object]，从最深层开始 |
| label | string | 请选择 | 默认显示文字 |
| dense | boolean | false              | 紧凑模式 |
| options | array   | []               | 必须，选项数组结构，参考示例 |
| option-label | string | label              | 数据结构中的名称，展示在控件中 |
| option-value | string | value  | 数据结构中的值 |
| @update:model-value | function(array, index) | -           | 选项变化时值传给父组件，参考示例 |
| no-caps | boolean | true | 是否禁用自动转为大写       |
| expand-icon | string  | keyboard_arrow_right | 展开icon         |
| index | integer | 0 | 第几个组件，当循环多个组件时使用 |

## 备注

* 最多支持 4 级，即只能有 3 层 children，参考示例。
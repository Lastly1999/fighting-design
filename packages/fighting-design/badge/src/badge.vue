<script lang="ts" setup name="FBadge">
  import { computed } from 'vue'
  import { Props } from './props'
  import { isNumber } from '../../_utils'
  import type { ComputedRef, CSSProperties } from 'vue'
  import type { ClassListInterface as a } from '../../_interface'
  import type { BadgePropsType } from './props'

  const prop: BadgePropsType = defineProps(Props)

  const classList: ComputedRef<a> = computed((): a => {
    const { type, dot } = prop

    return [
      'f-badge__content',
      {
        [`f-badge__${type}`]: type,
        'f-badge__dot': dot
      }
    ] as const
  })

  const content: ComputedRef<string> = computed((): string => {
    const { dot, max, value } = prop

    if (dot) return ''

    if (isNumber(max) && isNumber(value)) {
      return max > value ? `${value}` : `${max}+`
    }

    return `${value}`
  })

  const styleList: ComputedRef<CSSProperties> = computed((): CSSProperties => {
    const { color, textColor } = prop

    return {
      '--f-badge-background': color,
      '--f-badge-text-color': textColor
    } as CSSProperties
  })
</script>

<template>
  <div class="f-badge" :style="styleList">
    <slot />
    <sup v-show="!show && (content || dot)" :class="classList">
      {{ content }}
    </sup>
  </div>
</template>

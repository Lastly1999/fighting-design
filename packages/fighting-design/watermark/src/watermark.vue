<script lang="ts" setup name="FWatermark">
  import { Props } from './props'
  import { createWatermark } from '../../_utils'
  import { ref, onMounted } from 'vue'
  import { useFilterProps } from '../../_hooks'
  import type { Ref, CSSProperties } from 'vue'
  import type { WatermarkPropsType } from './props'
  import type { WatermarkCSSPropertiesInterface as a } from './interface'
  import type { CreateWatermarkPropsInterface as b } from '../../_interface'

  const prop: WatermarkPropsType = defineProps(Props)

  const watermarkStyleList: Ref<CSSProperties> = ref<CSSProperties>(
    null as unknown as CSSProperties
  )

  const baseWatermark: a = (): CSSProperties => {
    const needProps: b = useFilterProps<WatermarkPropsType, b>(prop, [
      'content',
      'width',
      'height',
      'fontSize',
      'fontColor'
    ])

    const watermark: string = createWatermark(needProps)

    return {
      backgroundImage: `url(${watermark})`
    } as CSSProperties
  }

  const imageWatermark: a = (): CSSProperties => {
    const { image, width, height } = prop

    return {
      backgroundImage: `url(${image})`,
      backgroundSize: `${width}px ${height}px`
    } as CSSProperties
  }

  onMounted((): void => {
    watermarkStyleList.value = prop.image ? imageWatermark() : baseWatermark()
  })
</script>

<template>
  <div
    :class="['f-watermark', { 'f-watermark__block': block }]"
    :style="[watermarkStyleList, { zIndex }]"
  >
    <slot />
  </div>
</template>

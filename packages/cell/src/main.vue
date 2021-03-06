<template lang="pug">
  .q-cell(
    @click="clicked"
    :style="computedStyle")
    .q-cell-left(
      v-if="!$slots.left&&(leftIcon||leftText)"
      :style="{width:leftWidth?leftWidth+'rem':'auto'}"
      @click.stop="leftClicked")
      //- 左侧icon
      .q-cell-left-icon(v-if="leftIcon")
        i(
          :class="leftIcon"
          :style="{color:leftIconColor}")
      //- 左侧文字
      .q-cell-left-text(v-if="leftText")
        span(:style="{color:leftTextColor}") {{leftText}}
    slot(name="left" v-if="$slots.left")
    //- 标题
    .q-cell-title(
      v-if="title&&!$slots.title")
      //- 标题文字
      .q-cell-title-text(
        :style="{color:titleColor}") {{title}}
    //- 自定义标题
    slot(name="title" v-if="$slots.title")
    .q-cell-right(
      v-if="!$slots.right&&(rightText||rightArrow)"
      @click.stop="rightClicked")
      //- 右侧描述
      .q-cell-right-text(
        v-if="rightText")
        span(:style="{color:rightTextColor}") {{rightText}}
      //- 右侧icon
      .q-cell-right-icon(
        v-if="rightArrow")
        i.q-icon.icon-right(:style="{color:rightArrowColor}")
    slot(name="right" v-if="$slots.right")
</template>

<script lang="ts">
import { Vue, Component, Prop, Emit } from 'vue-property-decorator'
import Proto from '../../proto/bar/main.vue'
import createStyle from '../../proto/bar'
const config = require('../../../src/qymhui.config').default.qcell

@Component({})
export default class QCell extends Proto {
  // 全局颜色
  @Prop({ default: config.bkColor })
  private bkColor: string

  // 是否有边距
  @Prop({ default: config.hasPadding })
  private hasPadding: boolean

  // 是否有上边线
  @Prop({ default: config.borderTop })
  private borderTop: boolean

  // 是否有下边线
  @Prop({ default: config.borderBottom })
  private borderBottom: boolean

  // border颜色
  @Prop({ default: config.borderColor })
  private borderColor: 'string'

  // 左侧icon
  @Prop({ default: config.leftIcon })
  private leftIcon: string

  // 左侧icon颜色
  @Prop({ default: config.leftIconColor })
  private leftIconColor: string

  // 左侧文字
  @Prop({ default: config.leftText })
  private leftText: string

  // 左侧文字颜色
  @Prop({ default: config.leftTextColor })
  private leftTextColor: string

  // 左侧宽度
  @Prop({ default: config.leftWidth })
  private leftWidth: string

  // title
  @Prop({ default: config.title })
  private title: string

  // title颜色
  @Prop({ default: config.titleColor })
  private titleColor: string

  // 右侧描述
  @Prop({ default: config.rightText })
  private rightText: string

  // 右侧文字颜色
  @Prop({ default: config.rightTextColor })
  private rightTextColor: string

  // 右侧箭头
  @Prop({ default: config.rightArrow })
  private rightArrow: boolean

  // 右侧箭头颜色
  @Prop({ default: config.rightArrowColor })
  private rightArrowColor: string

  // 计算样式
  private get computedStyle() {
    let style = createStyle(this)
    let selectableStyle = Object.create(null)
    selectableStyle.backgroundColor = this.bkColor
    selectableStyle.height = config.baseHeight + 'rem'
    if (this.borderTop) {
      selectableStyle.borderTopStyle = 'solid'
      selectableStyle.borderTopWidth = '1px'
      selectableStyle.borderTopColor = this.borderColor
    }
    if (this.borderBottom) {
      selectableStyle.borderBottomStyle = 'solid'
      selectableStyle.borderBottomWidth = '1px'
      selectableStyle.borderBottomColor = this.borderColor
    }
    if (this.hasPadding) {
      selectableStyle.padding = '0rem 0.2rem'
    }
    return Object.assign({}, style, selectableStyle)
  }

  @Emit('clicked')
  private clicked() {}

  @Emit('rightClicked')
  private rightClicked() {
  }

  @Emit('leftClicked')
  private leftClicked() {
  }

  private mounted() {
    this.$nextTick(() => {
      if (this.$slots.title) {
        let title: any = this.$slots.title[0].elm
        title.classList.add('q-cell-title')
      }
      if (this.$slots.left) {
        let left: any = this.$slots.left[0].elm
        left.classList.add('q-cell-left')
        left.addEventListener('click', this.leftClicked)
      }
      if (this.$slots.right) {
        let right: any = this.$slots.left[0].elm
        right.classList.add('q-cell-right')
        right.addEventListener('click', this.rightClicked)
      }
    })
  }
}
</script>

<style lang="scss" scoped>
.q-cell {
  display: flex;
  align-items: center;
  &-left {
    margin-right: 0.2rem;
    &-icon {
      display: flex;
      flex-grow: 0;
    }
  }
  &-title {
    height: 100%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    flex-grow: 1;
  }
  &-right {
    display: flex;
    flex-grow: 0;
    &-text {
      font-size: 13px;
      display: flex;
      align-items: center;
    }
    &-icon {
    }
  }
}
</style>

<template>
  <span :class="classes">
    <img :src="src" v-if="src">
    <i :class="icon" v-else-if="icon"></i>
    <span ref="children"
          :class="[prefixCls+'-string']"
          :style="childrenStyle" v-else>
      <slot></slot>
    </span>
  </span>
</template>

<script>
  const prefixCls = 'xdh-avatar'
  /**
   * XdhAvatar
   * @module widgets/xdh-avatar
   */
  export default {
    name: 'XdhAvatar',
    /**
     * props
     * @member props
     * @property {object} props
     * @property {string} [props.shape=circle] 头像现状，可选值：circle / square
     * @property {string} [props.size=default] 头像尺寸，可选值：small / large / default / xl / xxl
     * @property {string} [props.src] 图片资源地址
     * @property {string} [props.icon] 字体图标className， src和icon只能设置一个
     */
    props: {
      shape: {
        validator(value) {
          return ['circle', 'square'].includes(value)
        },
        default: 'circle'
      },
      size: {
        validator(value) {
          return ['small', 'large', 'default', 'xl', 'xxl'].includes(value)
        },
        default: 'default'
      },
      src: String,
      icon: String
    },
    data() {
      return {
        prefixCls: prefixCls,
        scale: 1,
        isSlotShow: false
      }
    },
    computed: {
      classes() {
        return [
          `${prefixCls}`,
          `${prefixCls}-${this.shape}`,
          `${prefixCls}-${this.size}`,
          {
            [`${prefixCls}-image`]: !!this.src,
            [`${prefixCls}-icon`]: !!this.icon
          }
        ]
      },
      childrenStyle() {
        let style = {}
        if (this.isSlotShow) {
          style = {
            msTransform: `scale(${this.scale})`,
            WebkitTransform: `scale(${this.scale})`,
            transform: `scale(${this.scale})`,
            position: 'absolute',
            display: 'inline-block',
            left: `calc(50% - ${Math.round(this.$refs.children.offsetWidth / 2)}px)`
          }
        }
        return style
      }
    },
    methods: {
      setScale() {
        this.isSlotShow = !this.src && !this.icon
        if (this.$refs.children) {
          const childrenWidth = this.$refs.children.offsetWidth
          const avatarWidth = this.$el.getBoundingClientRect().width
          // add 4px gap for each side to get better performance
          if (avatarWidth - 8 < childrenWidth) {
            this.scale = (avatarWidth - 8) / childrenWidth
          } else {
            this.scale = 1
          }
        }
      }
    },
    mounted() {
      this.setScale()
    },
    updated() {
      this.setScale()
    }
  }
</script>

<script>
  import utils from './../../utils/domUtils'

  const SHOW_CLASS = 'in'
  const BASE_CLASS = 'tooltip fade'

  export default {
    render (h) {
      return h(
        this.tag,
        [
          this.$slots.default,
          h('div',
            {
              ref: 'tooltip',
              attrs: {
                role: 'tooltip'
              },
              on: {
                mouseenter: this.showOnHover,
                mouseleave: this.hideOnLeave
              }
            },
            [
              h('div', {'class': 'tooltip-arrow'}),
              h('div', {
                'class': 'tooltip-inner',
                domProps: {innerHTML: this.text}
              })
            ]
          )
        ]
      )
    },
    props: {
      value: {
        type: Boolean,
        default: false
      },
      tag: {
        type: String,
        default: 'span'
      },
      text: {
        type: String,
        default: ''
      },
      trigger: {
        type: String,
        default: utils.triggers.HOVER_FOCUS
      },
      placement: {
        type: String,
        default: utils.placements.TOP
      },
      autoPlacement: {
        type: Boolean,
        default: true
      },
      appendTo: {
        type: String,
        default: 'body'
      },
      transitionDuration: {
        type: Number,
        default: 150
      },
      enable: {
        type: Boolean,
        default: true
      },
      target: {}
    },
    data () {
      return {
        timeoutId: 0,
        triggerEl: null
      }
    },
    mounted () {
      utils.ensureElementMatchesFunction()
      utils.removeFromDom(this.$refs.tooltip)
      this.$nextTick(() => {
        this.initTriggerElByTarget(this.target)
        this.initListeners()
        if (this.value) {
          this.show()
        }
      })
    },
    beforeDestroy () {
      this.clearListeners()
      utils.removeFromDom(this.$refs.tooltip)
    },
    watch: {
      value (v) {
        v ? this.show() : this.hide()
      },
      trigger () {
        this.clearListeners()
        this.initListeners()
      },
      target (value) {
        this.clearListeners()
        this.initTriggerElByTarget(value)
        this.initListeners()
      }
    },
    methods: {
      initTriggerElByTarget (target) {
        if (target) {
          // target exist
          if (typeof target === 'string') { // is selector
            this.triggerEl = document.querySelector(target)
          } else if (utils.isElement(target)) { // is element
            this.triggerEl = target
          } else if (utils.isElement(target.$el)) { // is component
            this.triggerEl = target.$el
          }
        } else {
          // use the first child
          let firstChild = this.$el.firstChild
          this.triggerEl = firstChild === this.$refs.tooltip ? null : firstChild
        }
      },
      initListeners () {
        if (this.triggerEl) {
          if (this.trigger === utils.triggers.HOVER) {
            utils.on(this.triggerEl, utils.events.MOUSE_ENTER, this.show)
            utils.on(this.triggerEl, utils.events.MOUSE_LEAVE, this.hide)
          } else if (this.trigger === utils.triggers.FOCUS) {
            utils.on(this.triggerEl, utils.events.FOCUS, this.show)
            utils.on(this.triggerEl, utils.events.BLUR, this.hide)
          } else if (this.trigger === utils.triggers.HOVER_FOCUS) {
            utils.on(this.triggerEl, utils.events.MOUSE_ENTER, this.handleAuto)
            utils.on(this.triggerEl, utils.events.MOUSE_LEAVE, this.handleAuto)
            utils.on(this.triggerEl, utils.events.FOCUS, this.handleAuto)
            utils.on(this.triggerEl, utils.events.BLUR, this.handleAuto)
          } else if (this.trigger === utils.triggers.CLICK || this.trigger === utils.triggers.OUTSIDE_CLICK) {
            utils.on(this.triggerEl, utils.events.CLICK, this.toggle)
          }
        }
        utils.on(window, utils.events.CLICK, this.windowClicked)
      },
      clearListeners () {
        if (this.triggerEl) {
          utils.off(this.triggerEl, utils.events.FOCUS, this.show)
          utils.off(this.triggerEl, utils.events.BLUR, this.hide)
          utils.off(this.triggerEl, utils.events.MOUSE_ENTER, this.show)
          utils.off(this.triggerEl, utils.events.MOUSE_LEAVE, this.hide)
          utils.off(this.triggerEl, utils.events.CLICK, this.toggle)
          utils.off(this.triggerEl, utils.events.MOUSE_ENTER, this.handleAuto)
          utils.off(this.triggerEl, utils.events.MOUSE_LEAVE, this.handleAuto)
          utils.off(this.triggerEl, utils.events.FOCUS, this.handleAuto)
          utils.off(this.triggerEl, utils.events.BLUR, this.handleAuto)
        }
        utils.off(window, utils.events.CLICK, this.windowClicked)
      },
      show () {
        let tooltip = this.$refs.tooltip
        if (!this.enable || !this.triggerEl || this.isShown()) {
          return
        }
        if (this.timeoutId > 0) {
          clearTimeout(this.timeoutId)
          this.timeoutId = 0
        } else {
          tooltip.className = `${BASE_CLASS} ${this.placement}`
          let container = document.querySelector(this.appendTo)
          container.appendChild(tooltip)
          utils.setTooltipPosition(tooltip, this.triggerEl, this.placement, this.autoPlacement, this.appendTo)
          tooltip.offsetHeight
        }
        utils.addClass(tooltip, SHOW_CLASS)
        this.$emit('input', true)
        this.$emit('show')
      },
      hide () {
        if (this.isShown()) {
          clearTimeout(this.timeoutId)
          utils.removeClass(this.$refs.tooltip, SHOW_CLASS)
          this.timeoutId = setTimeout(() => {
            utils.removeFromDom(this.$refs.tooltip)
            this.timeoutId = 0
            this.$emit('input', false)
            this.$emit('hide')
          }, this.transitionDuration)
        }
      },
      toggle () {
        if (this.isShown()) {
          this.hide()
        } else {
          this.show()
        }
      },
      showOnHover () {
        if (this.trigger === utils.triggers.HOVER || this.trigger === utils.triggers.HOVER_FOCUS) {
          this.show()
        }
      },
      hideOnLeave () {
        if (this.trigger === utils.triggers.HOVER || (this.trigger === utils.triggers.HOVER_FOCUS && !this.triggerEl.matches(':focus'))) {
          this.hide()
        }
      },
      handleAuto () {
        setTimeout(() => {
          if (this.triggerEl.matches(':hover, :focus')) {
            this.show()
          } else {
            this.hide()
          }
        }, 20) // 20ms make firefox happy
      },
      isShown () {
        return utils.hasClass(this.$refs.tooltip, SHOW_CLASS)
      },
      windowClicked (event) {
        if (this.triggerEl && !this.triggerEl.contains(event.target) &&
          this.trigger === utils.triggers.OUTSIDE_CLICK && !this.$refs.tooltip.contains(event.target) &&
          this.isShown()) {
          this.hide()
        }
      }
    }
  }
</script>

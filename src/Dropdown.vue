<template>
  <div class="btn-group">
    <slot></slot>
    <slot name="dropdown-menu"></slot>
  </div>
</template>
<script>
  import EventListener from './utils/EventListener'
  export default {
    methods: {
      toggleDropdown(e) {
        e.preventDefault()
        this.$el.classList.toggle('open')
        this.on = !this.on;
        this.$dispatch(this.on ? 'dropdown-open': 'dropdown-close', this)
      }
    },
    data() {
      return {
        _closeEvent: undefined,
        on: false
      }
    },
    ready() {
      const el = this.$el
      const that = this
      const toggle = el.querySelector('[data-toggle="dropdown"]')
      if (toggle)
      {
        toggle.style.borderRadius = '4px'
        toggle.addEventListener('click', this.toggleDropdown)
      }
      this._closeEvent = EventListener.listen(window, 'click', (e)=> {
        if (!el.contains(e.target) || e.target.nodeName.toLowerCase() == 'a') {
          if (that.on) {
            that.on = false
            el.classList.remove('open')
            that.$dispatch('dropdown-close', that)
          }
        }
      })
    },
    beforeDestroy() {
      if (this._closeEvent) this._closeEvent.remove()
    }
  }
</script>

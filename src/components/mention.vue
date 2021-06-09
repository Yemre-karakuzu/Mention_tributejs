<script>
import Tribute from 'tributejs'
export default {
  name: 'Mention',
  props: {
    options: {
      type: Object,
      required: true,
    },
  },
  watch: {
    options: {
      immediate: true,
      deep: true,
      handler() {
        this.$nextTick(() => {
          const $elm = this.$slots.default[0].elm
         
          console.log()
          this.tribute = new Tribute(this.options)
          if (this.tribute) {
            this.tribute.detach($elm)
          }

          this.tribute.attach($elm)
          $elm.tributeInstance = this.tribute
        })
      },
    },
  },
  beforeDestroy() {
    const $elm = this.$slots.default[0].elm

    if (this.tribute) {
      this.tribute.detach($elm)
    }
  },
  render(h) {
    return h(
      'div',
      {
        staticClass: 'v-mention',
      },
      this.$slots.default
    )
  },
}
</script>

<style>
.tribute-container {
  position: absolute;
  top: 0;
  left: 0;
  margin-top: 10px;
  max-height: 300px;
  max-width: 500px;
  overflow: auto;
  display: block;
  z-index: 999999;
  border-radius: 4px;
  box-shadow: 0 1px 3px 0 rgba(0, 0, 0, 0.1), 0 1px 2px 0 rgba(0, 0, 0, 0.06);
}
.tribute-container ul {
  margin: 0 0;
  padding: 8px 0;
  list-style: none;
  background: #fff;
  border-radius: 4px;
  overflow: hidden;
}
.tribute-container li {
  display: flex;
  align-items: center;
  color: #4a4a4a;
  padding: 8px 16px;
  white-space: nowrap;
  cursor: pointer;
  font-size: 12px;
}
.tribute-container li.highlight,
.tribute-container li:hover {
  background: #eaf1f9;
}
.tribute-container li span {
  font-weight: normal;
}
.tribute-container li.no-match {
  cursor: default;
}
.tribute-container .menu-highlighted {
  font-weight: bold;
}
</style>

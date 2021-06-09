<script>
import Mention from './mention'



const placeholder = (item) => {
  const firstName = item.original.username.charAt(0)
  const lastName =
    item.original.username.charAt(1) === '.'
      ? item.original.username.charAt(2)
      : item.original.username.charAt(1)
  const nameAvt = firstName + lastName
  return item.original.imageFolder === ''
    ? `${nameAvt.trim()}`
    : ''
}

export default {
  name: 'Editor',
  components: {
    'at-mention': Mention,
  },
  props: {
    value: {
      type: String,
      required: true,
      default: '',
    },
    placeholder: {
      type: String,
      default: '',
    },
    users: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    let mentionlist = []
    return {
      isFocused: false,
      editorText: '',
      mentions: mentionlist,
      options: {
        trigger: '@',
        allowSpaces: true,
        values: [],
        positionMenu: true,
        menuContainer: null,
        autocompleteMode: false,
        lookup: function(person) {
          return person.name + ' ' + person.lastName + ' ' + person.username
        },
        fillAttr: 'username',
        itemClass: 'menu-item',
        selectTemplate: function(item) {
          const inList =
            mentionlist.length &&
            mentionlist.some((mention) => {
              return mention.original.username === item.original.username
            })
          if (inList) {
            return ''
          } else {
            mentionlist.push(item)
          }

          if (typeof item === 'undefined') return null
          if (this.range.isContentEditable(this.current.element)) {
            return (
              '<span data-id="' +
              item.index +
              '" contenteditable="false"><a class="text-blue-900 font-semibold">' +
              item.original.name +
              ' ' +
              item.original.lastName +
              '</a></span>'
            )
          }
          return item.original.username
        },
        menuItemTemplate: function(item) {
          return (
            `<span role="img" style="${(
              item
            )}" class="w-6 h-6 mr-2 font-thin border border-gray-900 bg-no-repeat bg-cover text-xs uppercase flex items-center justify-center text-white rounded-full relative cursor-pointer">${placeholder(
              item
            )}</span>` +
            item.original.name +
            ' ' +
            item.original.lastName +
            '<span class="ml-3 text-xxs text-gray-500">' +
            item.original.username +
            '</span>'
          )
        },
      },
    }
  },
  watch: {
    users: {
      immediate: true,
      handler(list) {
        this.options.values = list
      },
    },
    value: {
      immediate: true,
      handler(comment) {
        this.editorText = comment
        if (comment === '' && this.$refs['editor-input']) {
          this.$refs['editor-input'].innerText = comment
        }
      },
    },
    mentions() {
      this.message()
    },
    editorText(comment) {
      this.mentions.map((mention, i) => {
        if (
          !comment.includes(
            `${mention.original.name} ${mention.original.lastName}`
          )
        ) {
          this.mentions.splice(i, 1)
        }
      })
    },
  },
  created() {
    document.addEventListener('click', this.documentClick)
  },
  destroyed() {
    document.removeEventListener('click', this.documentClick)
  },
  methods: {
    documentClick(e) {
      if (!this.$el.contains(e.target)) {
        this.isFocused = false
        this.$refs['editor-input'].blur()
      }
    },
    message() {
      this.editorText = this.$refs['editor-input'].innerText.trim()
      this.$emit('input', this.editorText)
      this.$emit('mentions', this.mentions)
    },
    focusIn() {
      this.isFocused = true
      this.$refs['editor-input'].focus()
    },
  },
}
</script>

<template>
  <div
    ref="editor"
    class="border bg-blue-200 rounded overflow-hidden hover:border-blue-500 focus-within:border-blue-900"
    :class="{ 'editor-focused': isFocused }"
    @click="focusIn"
  >
    <at-mention
      :options="options"
      :style="{
        height: $slots.toolbar && isFocused ? `calc(100% - 56px)` : '100%',
      }"
    >
      <div
        ref="editor-input"
        contenteditable="true"
        tabindex="0"
        :class="{ 'is-focused': isFocused }"
        :placeholder="placeholder"
        class="editor-input h-full w-full h-ease-15 bg-transparent text-xs overflow-auto px-3 py-3 outline-none"
        @keyup="message"
      />
    </at-mention>
    <slot name="toolbar"></slot>
  </div>
</template>

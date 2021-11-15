<template>
  <A :checked.sync="checked" />
</template>
<script>

export default ({
  data() {
    global.t = this
    return {
      checked: true
    }
  },
  components: {
    A: {
      render() {
        global.a = this
        return <input
          type="checkbox"
          checked={this.checked}
          onchange={() => {
            this.$emit('update:checked', !this.checked)
          }}
        />
      },
      props: {
        checked: Boolean
      },

    }
  },
})
</script>

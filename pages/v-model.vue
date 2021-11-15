<template>
  <A v-model="checked" />
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
          onchange={($event) => {
            this.$emit('*', $event.target.checked)
          }}
        />
      },
      model: {
        prop: 'checked',
        event: '*'
      },
      props: ['checked']
    }
  },
})
</script>

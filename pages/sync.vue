<template>
  <B :list.sync="list" />
</template>
<script>

export default ({
  data() {
    global.t = this
    return {
      checked: true,
      list: [{ name: 'name' }]
    }
  },
  components: {
    B: {
      render() {
        global.b = this
        return <div>
          <button onclick={() => {
            let t = [...this.list, { name: new Date() + '' }]
            // this.list = t
            this.$emit('update:list', t)
          }}>add</button>
          <ul>
            {this.list.map((a, i) => <li key={i}>{a.name}</li>)}
          </ul>
        </div>
      },
      props: ['list'],
    },
  },
})
</script>

<template>

</template>

<script>
export default {
  data () {
    const mark = ['heart', 'dia', 'clover', 'spade']
    const num = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13]
    const surface = ['true', 'false']
    const trumps = []
      for (let i = mark.length -1 ; i > 0; i--) {
        for (let l = num.length -1 ; l > 0; l--) {
          const k = Math.floor(Math.random() * (i + 1))
          const markRandom = mark[i]
          mark[i] = mark[k]
          mark[k] = markRandom
          console.log(mark)
          // trumps.push(mark[k])
      
          const m = Math.floor(Math.random() * (l + 1))
          const numRandom = num[l]
          num[l] = num[m]
          num[m] = numRandom
          
        console.log(num)
        // trumps.push(num)
        }
      }
    return {
    }
  },
  methods: {
  }         
}
</script>

<style>

</style>
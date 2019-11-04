<template>
  <div class="consentration-box">
    <div v-for="n in trumps.length" 
    :key="n"
    class="trumpsShape">
   </div>
 </div>
</template>

<script>
export default {
  data () {
    const mark = ['heart', 'dia', 'clover', 'spade']
    const trumps = []
    console.log(trumps)
      for (let i = 0; i < 4; i++) {
        for (let k = 1; k <= 13; k++) {
        trumps.push({trumpMark: mark[i],trumpNum: k,surface: false})
        }
      }
      for (let h = trumps.length -1; h >= 0; h--) {
        const m = Math.floor(Math.random() * (h + 1))
        const tmp = trumps[h]
        trumps[h] = trumps[m]
        trumps[m] = tmp
        console.log(trumps[h])
        }
    return {
      trumps :trumps  // trumpsプロパティをもつ連想配列 
    }
  },
  methods: {
  }         
}
</script>

<style>
.consentration-box{
  overflow: hidden;
  width: 1190px;
  background-color: green;
  margin: 50px auto;
  padding: 10px;
}
.trumpsShape{
  height: 120px;
  width: 80px;
  margin: 5px;
  box-sizing: border-box;
  float: left;
  background-image: url("~assets/images/card_back.PNG");
  background-size: cover;
}
</style>
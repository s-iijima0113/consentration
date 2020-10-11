<template>
  <div class="consentration-box">
    <div v-for="(trump, i) in trumps" 
      v-bind:key="i">
    </div>
      <div @click="clickTrump(trump)">
        <div v-if="isActive === false">
          <img class="trumpShape" src="@/assets/backcard/card_back.png">
        </div>
        <div v-else>
          <img class="trumpShape" src="@/assets/images/c01.gif">
        </div>
      </div>
    <!-- ①trumpsの配列を引っ張ってきている。 -->
    <!-- ②trumpで①の配列の中身（連想配列）を引っ張っている -->
    <!-- ③i は代入するもの。（配列の番号）（文字は何でも良い） -->
    <!-- ①trumpsの配列を引っ張ってきている。 -->
    <!-- ④@clickでクリックしたらisSurfaceをmethodsのisSurfaceに渡している -->
      <!--<div v-if="trump.surface === true">--> 
        <!--<img :src="~assets/images/trumpImage">-->
      <!--</div> -->
      <!--<div v-else> --> 
        <!--<img src="~assets/backcard/card_back.png">-->
      <!--</div>-->   
  </div>
</template>

<script>
export default {
  data () {
    const mark = ['h', 'd', 'c', 's']
    const trumps = []
    // console.log(trumps)
      for (let i = 0; i < 4; i++) {
        for (let k = 1; k <= 13; k++) {
        trumps.push({trumpMark: mark[i],trumpNum: k,surface: false})
        }
      }
      for (let h = trumps.length -1; h >= 0; h--) {
        const m = Math.floor(Math.random() * (h + 1))
        // 乱数を取得
        const tmp = trumps[h]
        trumps[h] = trumps[m]
        trumps[m] = tmp
        // console.log(trumps[h])
        }
    return {
      trumps :trumps,  // trumpsプロパティをもつ連想配列
      isActive: false
    }
  },
  methods: {
    clickTrump(trump) {
      this.isActive = !this.isActive
      console.log(trump)
      const trumpImage = []
      console.log(trumpImage)    
      //if (trump.surface === false) {
        //trump.surface = true
      //} if (trump.trumpNum < 10) {
        //trumpImage.push([trump.trumpMark+'0'+trump.trumpNum])
      //} else {
        //trumpImage.push([trump.trumpMark+trump.trumpNum])
      //}
      // if (trump.surface === true) {
        // <img src="~assets/images/trumpImage">
      // }
    }
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
.trumpShape{
  height: 120px;
  width: 80px;
  margin: 5px;
  box-sizing: border-box;
  float: left;
  background-size: cover;
}
</style>
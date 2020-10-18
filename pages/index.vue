<template>
  <div class="consentration-box">
    <div v-for="(trump, i) in trumps" v-bind:key="i">
      <img
        v-bind:src="trump.isOpen ? trump.trumpInfo.front : trump.trumpInfo.back"
        class="trumpShape"
        @click="clickTrump(i)"
      />
    </div>
    <!--div v-if="isActive === false"-->
    <!--img class="trumpShape" src="@/assets/backcard/card_back.png"-->
    <!--/div-->
    <!--div v-else-->
    <!--img class="trumpShape" src="@/assets/images/c01.gif"-->
    <!--/div-->
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
  data() {
    const trumps = [];
    const marks = ["h", "d", "c", "s"];
    //console.log(trumps)
    for (let m = 0; m < 4; m++) {
      for (let k = 1; k <= 13; k++) {
        let numbers = (`00` + k).slice(-2);
        let trump = {
          isOpen: false, //初期はfalse
          trumpInfo: {
            mark: marks[m],
            number: numbers,
            front: require(`@/assets/images/${marks[m]}${numbers}.gif`), //裏
            back: require("@/assets/backcard/card_back.png"), //表
          },
        };
        trumps.push(trump);
      }
    }
    for (let h = trumps.length - 1; h >= 0; h--) {
      const m = Math.floor(Math.random() * (h + 1));
      // 乱数を取得
      const tmp = trumps[h];
      trumps[h] = trumps[m];
      trumps[m] = tmp;
    }
    // if (this.lastFlippedTrumpIndex === i) {
    //   this.trumps[i].isOpen = true;
    // } else {
    //   this.trumps[i].isOpen = false;
    // }

    return {
      trumps: trumps, // trumpsプロパティをもつ連想配列
      lastFlippedTrumpIndex: undefined,
    };
  },
  methods: {
    clickTrump: function (i) {
      //カードを表向きにする
      // console.log(this.lastFlippedTrumpIndex);
      // 1: undefined
      // 2: 2
      // この下からdata()のreturnを返してくる。lastFlippedTrumpIndexにiを代入している。
      this.lastFlippedTrumpIndex = i;
      // 1: 2
      // 2: 3
      // 何番目のトランプをクリックしたかわかる
      // console.log(i);
      let firstTrumpClick = this.trumps[i];
      console.log(this.trumps[i]);
      // let hoge = this.trumps[i];
      this.trumps[i].isOpen = true;
      // console.log(this.trumps[i]);
      // const trumpImage = []
      // console.log(trumpImage)
      // if (trump.surface === false) {
      //trump.surface = true
      //if (trump.trumpNum < 10) {
      //trumpImage.push([trump.trumpMark+'0'+trump.trumpNum])
      //} else {
      //trumpImage.push([trump.trumpMark+trump.trumpNum])
      //}
      // if (trump.surface === true) {
      // <img src="~assets/images/trumpImage">
      // }
    },
  },
};
</script>

<style>
.consentration-box {
  overflow: hidden;
  width: 1190px;
  background-color: green;
  margin: 50px auto;
  padding: 10px;
}
.trumpShape {
  height: 120px;
  width: 80px;
  margin: 5px;
  box-sizing: border-box;
  float: left;
  background-size: cover;
}
</style>

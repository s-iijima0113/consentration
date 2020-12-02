<template>
  <div class="consentration-box">
    <div v-for="(trump, i) in trumps" v-bind:key="i">
      <img
        v-bind:src="
          trump.isOpen || trump.isGet != null
            ? trump.trumpInfo.front
            : trump.trumpInfo.back
        "
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
//クリックしたトランプの数を把握
let openedTrump = 0;
const PLAYER = "Player";
export default {
  data() {
    return {
      trumps: [],
    };
  },
  mounted() {
    const marks = ["h", "d", "c", "s"];
    for (let m = 0; m < 4; m++) {
      for (let k = 1; k <= 13; k++) {
        let numbers = (`00` + k).slice(-2);
        let trump = {
          isOpen: false, //初期はfalse
          isGet: null,
          trumpInfo: {
            mark: marks[m],
            number: numbers,
            front: require(`@/assets/images/${marks[m]}${numbers}.gif`), //裏
            back: require("@/assets/backcard/card_back.png"), //表
          },
        };
        this.trumps.push(trump);
      }
    }
    for (let h = this.trumps.length - 1; h >= 0; h--) {
      const m = Math.floor(Math.random() * (h + 1));
      // 乱数を取得
      const tmp = this.trumps[h];
      this.trumps[h] = this.trumps[m];
      this.trumps[m] = tmp;
    }
    return {
      trumps: this.trumps, // trumpsプロパティをもつ連想配列
      // trumps: [], // clickedTrumps: [],
    };
  },

  methods: {
    clickTrump: function (i) {
      if (openedTrump >= 2) return;
      // const trumpImage = []
      // 1: undefined
      // 2: 2
      // この下からdata()のreturnを返してくる。lastFlippedTrumpIndexにiを代入している。
      // 1: 2
      // 2: 3
      // console.log(i);
      // 何番目のトランプをクリックしたかわかる
      // let firstTrumpClick = this.trumps[i];
      // let hoge = this.trumps[i];

      // const secondClickedTrump = this.trumps[i];
      // const firstClickedTrump = this.trumps[this.lastFlippedTrumpIndex];
      //2枚以上はめくれないようにする
      // if (openedTrump + 1 > 2) return;
      // //1枚目を表にする
      // // if (firstClickedTrump == 1) {
      // const clickedTrumps = [];
      // clickedTrumps.push(i);
      this.trumps[i].isOpen = true;
      openedTrump++;
      // console.log(openedTrump);
      if (openedTrump == 2) {
        this.isNumMatch();
        // this.reset();
      }
    },
    // isNumMatch: function (i) {},

    reset: function () {
      setTimeout(() => {
        this.trumps = this.trumps.map(function (trump) {
          if (trump.isOpen) {
            return { ...trump, isOpen: false }; //{...trumpでtrumpのコピーオブジェクトができて、,isOpen: false}で部分的に上書きできる
          } else {
            return trump; //処理に関係ないtrumpはコピーせずにreturn
          }
        });
        openedTrump = 0;
      }, 3000);
    },

    // reset: function () {
    //   setTimeout(
    //     function () {
    //       this.trumps.forEach(function (trump, i) {
    //         if (trump.isOpen) {
    //           this.trump.isOpen = false;
    //         }
    //       });
    //       // console.log(this.trump);
    //       // firstClickedTrump.isOpen = false;
    //       // console.log(openedTrump);
    //     }.bind(this),
    //     1000
    //   );
    //   openedTrump = 0;
    // },
    // //2枚目を表にする
    //   if (firstClickedTrump != 1) {
    //     secondClickedTrump.isOpen = true;
    //     openedTrump++;
    //     console.log(openedTrump);
    //     // console.log(secondClickedTrump.trumpInfo.number);
    //     console.log("2枚目のトランプを表にする");
    //     // console.log(this.lastFlippedTrumpIndex);
    //     this.isNumMatch(i);
    //   }
    //   this.lastFlippedTrumpIndex = i;
    // },

    isNumMatch: function () {
      const clickedTrumps = [];
      this.trumps.forEach((trump, i) => {
        if (trump.isOpen && trump.isGet == null) {
          clickedTrumps.push({ i, trump });
        }
      });
      console.log(clickedTrumps);
      const firstClickedTrump = clickedTrumps[0];
      const secondClickedTrump = clickedTrumps[1];
      console.log(firstClickedTrump);
      console.log(secondClickedTrump);

      if (
        firstClickedTrump.trump.trumpInfo.number ===
        secondClickedTrump.trump.trumpInfo.number
      ) {
        console.log("同じ番号だよ！");
        //ずっとカードを表にする
        // secondClickedTrump.isGet = PLAYER;
        // firstClickedTrump.isGet = PLAYER;
        // } else {
        //   setTimeout(function () {
        //     console.log("違う番号だよ！");
        //     secondClickedTrump.isOpen = false;
        //     firstClickedTrump.isOpen = false;
        //   }, 1000);
        //   // 1秒間トランプの表を表示した後裏にturnする
      } else {
        this.reset();
      }
    },

    // reset: function (i) {
    //   const secondClickedTrump = this.trumps[i];
    //   const firstClickedTrump = this.trumps[this.lastFlippedTrumpIndex];
    //   setTimeout(function () {
    //     console.log("違う番号だよ！");
    //     secondClickedTrump.isOpen = false;
    //     firstClickedTrump.isOpen = false;
    //     console.log(openedTrump);
    //   }, 2000);
    //   // 1秒間トランプの表を表示した後裏にturnする
    //   openedTrump = 0;
    // },

    // console.log(secondClickedTrump.isOpen);
    // }
    // }

    // console.log(secondClickedTrump.trumpInfo.number);

    // console.log(this.trumps[i].trumpInfo.number);
    // if (this.trumps[this.lastFlippedTrumpIndex] === this.trumps[i]) {
    //   //トランプ表のまま
    //   console.log(firstClickedTrump.trumpInfo.number);
    //   this.trumps[i].isOpen = true;
    // } else {
    //   //トランプを返す
    //   this.trumps[i].isOpen = false;
    // }
    // this.lastFlippedTrumpIndex = i;

    //カードを表向きにする
    // this.trumps[i].isOpen = true;
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

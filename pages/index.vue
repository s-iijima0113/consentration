<template>
  <div class="getTrumpCount">
    あなたが取得した組数は〇〇組です
    <div class="consentration-box">
      <!-- <div class="getTrumps">あなたが取得したトランプは〇〇組です</div> -->
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
      <!-- ①trumpsの配列を引っ張ってきている。 -->
      <!-- ②trumpで①の配列の中身（連想配列）を引っ張っている -->
      <!-- ③i は代入するもの。（配列の番号）（文字は何でも良い） -->
      <!-- ①trumpsの配列を引っ張ってきている。 -->
      <!-- ④@clickでクリックしたらisSurfaceをmethodsのisSurfaceに渡している -->
    </div>
  </div>
</template>

<script>
//クリックしたトランプの数を把握
let openedTrump = 0;
const PLAYER = "Player";
const COMPUTER = "computer";

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
    };
  },

  methods: {
    clickTrump: function (i) {
      if (openedTrump >= 2) return;
      if (this.trumps[i].isOpen == true) return;
      this.trumps[i].isOpen = true;
      openedTrump++;

      if (openedTrump == 2) {
        this.isNumMatch();
      }
    },

    reset: function () {
      setTimeout(() => {
        this.trumps = this.trumps.map(function (trump) {
          if (trump.isOpen && trump.isGet == null) {
            return { ...trump, isOpen: false }; //{...trumpでtrumpのコピーオブジェクトができて、,isOpen: false}で部分的に上書きできる
          } else {
            return trump; //処理に関係ないtrumpはコピーせずにreturn
          }
        });
        openedTrump = 0;
      }, 2000);
    },

    isNumMatch: function () {
      const clickedTrumps = [];
      this.trumps.forEach((trump, i) => {
        if (trump.isOpen && trump.isGet == null) {
          clickedTrumps.push({ i, trump });
        }
      });
      // console.log(clickedTrumps);
      const firstClickedTrump = clickedTrumps[0];
      const secondClickedTrump = clickedTrumps[1];
      console.log(firstClickedTrump);
      console.log(secondClickedTrump);

      if (
        firstClickedTrump.trump.trumpInfo.number ===
        secondClickedTrump.trump.trumpInfo.number
      ) {
        // console.log("同じ番号だよ！");
        // console.log(firstClickedTrump.trump.isGet);
        firstClickedTrump.trump.isGet = PLAYER;
        secondClickedTrump.trump.isGet = PLAYER;
        this.reset();
      } else {
        this.reset();
      }
    },
  },
};
</script>

<style>
.getTrumpCount {
  text-align: center;
  margin-top: 5px;
}
.consentration-box {
  overflow: hidden;
  width: 1190px;
  background-color: green;
  margin: 30px auto;
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

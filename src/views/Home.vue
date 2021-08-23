<template>
  <div class="home">
    <div class="header">
      Matching Game
      <div class="selete"><span @click="handelFour">4*4</span> <span @click="handelEight">6*6</span></div>
    </div>
    <div class="main">
      <ul class="gameBoard">
        <my-card :class="classState ? 'Eight' : 'Four'" :title="title" cover="❓" :isShow="isOpen[index]" v-for="(title, index) of this.cardSymbolsInRands" :key="index" @click.native="handelPress(index)"></my-card>
      </ul>
    </div>
    <div class="footer">
      {{ this.isEnded ? `Congrats! You have tried ${this.steps} steps.` : `You have tried ${this.steps} time(s)` }}
      <div class="TryAgain" v-show="this.isEnded == true" @click="resetGame"><span>Try Again</span></div>
    </div>
    <!-- <div class="footer"></div> -->
  </div>
</template>

<script>
import Card from '../component/Card.vue'
export default {
  data() {
    return {
      cardSymbols: ['😋', '😍', '😑', '🤑', '💔', '💩', '🤡', '🤬'],
      cardSymbolsEight: ['😋', '😍', '😑', '🤑', '💔', '💩', '🤡', '🤬', '🤐', '🐷', '🤣', '💋', '😎', '🤔', '👀', '👍', '😶', '😭'],
      cardSymbolsInRands: [], //用来储存随机生成的emoji  => 16个
      newList: '',
      isOpen: [], //存储 true  false
      firstPickedIndex: null, //存储前一次点击   index
      secondPickedIndex: null, //存储下一次点击  index
      steps: 0, //记录点击多少次
      isEnded: false, //判断是否结束
      classState: false,
    }
  },
  methods: {
    handelFour() {
      this.classState = false
      this.isEnded = false
      this.initGame(this.cardSymbols)
    },
    handelEight() {
      this.classState = true
      this.isEnded = false
      this.initGame(this.cardSymbolsEight)
    },
    shuffleArray(arr) {
      const newArr = arr.slice() //复制 newCardSymbols
      //打乱顺序
      for (let i = newArr.length - 1; i > 0; i--) {
        const rand = Math.floor(Math.random() * (i + 1))
        ;[newArr[i], newArr[rand]] = [newArr[rand], newArr[i]]
      }
      return newArr
    },
    handelPress(index) {
      let isOpen = [...this.isOpen]

      if (isOpen[index]) {
        // 卡牌已打开 不执行后续
        return
      }

      isOpen[index] = true
      //说明是第一次点击
      if (this.firstPickedIndex == null && this.secondPickedIndex == null) {
        this.isOpen = isOpen
        this.firstPickedIndex = index
      } else if (this.firstPickedIndex != null && this.secondPickedIndex == null) {
        //说明是第二次点击
        this.isOpen = isOpen
        this.secondPickedIndex = index
      }
      // 点击加 1
      this.steps += 1
      // this.calculateGameResult()
    },
    calculateGameResult() {
      //判断翻开的两张卡牌是否一样
      if (this.firstPickedIndex != null && this.secondPickedIndex != null) {
        if (this.cardSymbolsInRands.length > 0) {
          //回圈  返回true
          let tatalOpens = this.isOpen.filter((isOpen) => isOpen)
          if (tatalOpens.length == this.cardSymbolsInRands.length) {
            this.isEnded = true
            // 结束后 不执行后续代码
            return
          }
        }

        let firstSymbol = this.cardSymbolsInRands[this.firstPickedIndex] //拿第一个值
        let secondSymbol = this.cardSymbolsInRands[this.secondPickedIndex] //拿第二个值

        if (firstSymbol !== secondSymbol) {
          //两张卡牌不一样
          // 卡牌翻转回去 延迟1秒
          setTimeout(() => {
            let isOpen = [...this.isOpen]
            // 卡牌不一样 转回去
            isOpen[this.firstPickedIndex] = false
            isOpen[this.secondPickedIndex] = false
            // 初始  让用户继续点击
            this.firstPickedIndex = null
            this.secondPickedIndex = null
            this.isOpen = isOpen
          }, 500)
        } else {
          //两张卡牌一样
          //用户可以继续翻转其他卡牌
          this.firstPickedIndex = null
          this.secondPickedIndex = null
        }
      }
    },
    initGame(cardarr) {
      let newCardSymbols = [...cardarr, ...cardarr] //把 cardSymbols 复制
      let CardSymbolsInRands = this.shuffleArray(newCardSymbols)
      this.cardSymbolsInRands = CardSymbolsInRands
      this.cardSymbolsInRands.map((Symbol) => (this.title = Symbol))
      let isOpen = []
      for (let i = 0; i < newCardSymbols.length; i++) {
        isOpen.push(false)
      }
      this.isOpen = isOpen
    },
    resetGame() {
      //初始化
      this.classState ? this.initGame(this.cardSymbolsEight) : this.initGame(this.cardSymbols)
      this.firstPickedIndex = null
      this.secondPickedIndex = null
      this.steps = 0
      this.isEnded = false
      console.log(132)
    },
  },
  watch: {
    secondPickedIndex: {
      //拿到最新值
      handler(newIndex, oldIndex) {
        if (newIndex != oldIndex) {
          this.calculateGameResult()
        }
      },
    },
  },
  mounted() {
    // this.initGame()
    this.initGame(this.cardSymbols)
  },
  components: {
    'my-card': Card,
  },
}
</script>
<style scoped>
.selete {
  font-size: 0.2rem;
}
.selete span {
  padding: 0.08rem;
  border-radius: 0.08rem;
  margin: 0 0.4rem;
  background-color: #cecece;
}
.TryAgain {
  background-color: #cecece;
  padding: 0.08rem;
  border-radius: 0.08rem;
  margin-top: 0.2rem;
  font-size: 0.18rem;
}
.gameBoard {
  display: flex;
  flex-wrap: wrap;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  min-width: 375px;
  max-width: 375px;
  margin: 0 auto;
  height: 100%;
}
.home {
  display: flex;
  flex-direction: column;
  flex: 1;
  margin: 0;
  padding: 0;
  background-color: #f0f;
  width: 100vw;
  height: 100vh;
}
.header {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-around;
  font-size: 0.48rem;
  font-weight: bold;
  background-color: #eee;
}
.main {
  flex: 3;
  background-color: #fff;
}
.footer {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  font-size: 0.22rem;
  background-color: #eee;
}
</style>

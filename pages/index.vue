<template>
  <main>
    <div class="timer" v-if="timerActive" :class="{'is-finished': currentIntervalTime <= 0}">
      <div class="timer-container pa1">
        <div class="close-timer pa1">
          <button class="button button--light" @click="closeTimer">&#10005;</button>
        </div>
        <h1 class="title pa1" v-if="currentIntervalTime > 0">
          <span class="minutes">{{ minutes }}</span>
          <span class="middle">:</span>
          <span class="seconds">{{ seconds }}</span>
        </h1>
        <h1 class="title pa1" v-else>{{ randomMessage.message }}</h1>
        <div class="timer-controls">
          <div class="timer-controls-container pa1">
            <button class="button button--light ma-gutter" @click="startTimer">Start</button>
            <button class="button button--light ma-gutter" @click="stopTimer">Stop</button>
            <button class="button button--light ma-gutter" @click="resetTimer">Reset</button>
          </div>
        </div>
      </div>
    </div>
    <div v-else class="intervals">
      <!-- <div class="interval-title">
        <p class="t-center pa2">{{ title }}</p>
      </div> -->
      <div class="intervals-container pa-gutter">
        <button 
          v-for="item in intervals" 
          :key="item.id"
          @click="setTimer(item.time)" 
          class="button button--dark pa-gutter"
          >{{ item.time }}</button>
      </div>
    </div>
  </main>
</template>

<script>
  export default {
    data: () => ({
      timer: null,
      storedIntervalTime: null,
      currentIntervalTime: null,
      title: "Select interval",
      timerActive: false,
      intervals: [
        {time: 30},
        {time: 45},
        {time: 60},
        {time: 90},
        {time: 120},
        {time: 150},
      ],
      finishedMessages: [
        {message: "Fini!", lang: "french"},
        {message: "Finished!", lang: "english"},
        {message: "Finito!", lang: "italian"},
        {message: "Terminado!", lang: "spanish"}
      ],
      randomMessage: '',
    }),
    methods: {
      getRandomMessage: function(){
        let randomNumber = Math.floor(Math.random() * this.finishedMessages.length);
        this.randomMessage = this.finishedMessages[randomNumber];
        console.log(this.randomMessage);
      },
      setTimer: function(value) {
        this.storedIntervalTime = value;
        this.currentIntervalTime = value;
        this.timerActive = !this.timerActive;
        // this.randomMessage = '';
        this.getRandomMessage();
      },
      startTimer: function() {
        this.timer = setInterval(() => this.countdown(), 1000);
        this.title = "Let's go!";
        this.getRandomMessage();
      },
      stopTimer: function() {
        clearInterval(this.timer);
        this.timer = null;
        this.title = "Never quit, keep going!";
      },
      resetTimer: function() {
        this.currentIntervalTime = this.storedIntervalTime;
        clearInterval(this.timer);
        this.timer = null;
        this.title = "Let the countdown begin!";
        // this.randomMessage = '';
        this.getRandomMessage();
      },
      closeTimer: function() {
        this.timerActive = !this.timerActive;
        this.storedIntervalTime = null;
        this.currentIntervalTime = null;
        clearInterval(this.timer);
        this.timer = null;
        this.resetButton = false;
        this.randomMessage = '';
      },
      padTime: function(time) {
        return (time < 10 ? '0' : '') + time;
      },
      countdown: function() {
        if(this.currentIntervalTime >= 1){
          this.currentIntervalTime--;
        } else{
          this.currentIntervalTime = 0;
          // this.resetTimer()
        }
      },
    },
    computed: {
      minutes: function() {
        const minutes = Math.floor(this.currentIntervalTime / 60);
        return this.padTime(minutes);
      },
      seconds: function() {
        const seconds = this.currentIntervalTime - (this.minutes * 60);
        return this.padTime(seconds);
      },
      // finishedMessage: function () {
      //   let randomMessage = Math.floor(Math.random() * this.finishedMessages.length);
      //   return this.finishedMessages[randomMessage];
      // }
    },
    components: { 
      // vue100vh 
    },
  }
</script>

<style lang="scss">
main{
 height: 100%;
 width: 100%;
 position: relative; 
}
.timer{
  height: 100%;
  width: 100%;
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  z-index: 100;
  background-color: var(--color-purple);
  .timer-container{
    margin: 0 auto;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-content: center;
    width: 100%;
    height: 100%;
    .timer-controls{
      position: fixed;
      bottom: 0;
      left: 0;
      right: 0;
      width: 100%;
      .timer-controls-container{
        display: flex;
        flex-direction: row;
        justify-content: center;
          .button{
            background-color: var(--color-light-navy);
            &:hover{
              background-color: var(--color-dark-navy);
            }
          }
      }
    }
  }
}

.timer.is-finished{
  background-color: var(--color-lime);
}

.title{
  width:100%;
}

.close-timer{
  position: fixed;
  top: 0;
  right: 0;
}

.intervals{
  position: fixed;
  height: 100%;
  width: 100%;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
}
.intervals-container {
  position: relative;
  margin: 0 auto;
  height: 100%;
  width: 100%;
  display: flex;
  flex-direction: column;
  .button{
    flex: 1;
    margin: calc(var(--padding) / 2);
    font-size: calc(1.6rem + 2vw);
  }
}

</style>


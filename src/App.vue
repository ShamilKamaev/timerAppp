<template>
  <section class="time-app-cont">
    <div class="time-app" id="timeApp">
      <div class="current-timer-descr">
        <input type="text" v-model="cuurentTimerDescr" placeholder="Опсиание таймера">
        <button>Сохранить</button>
      </div>

      <div class="current-time">
        <p>{{ currentTime }}</p>
      </div>

      <div>
        <button class="start-stop-btn"
          :class="{'is-active': timerIsActive}"
          @click="startStopTimer">{{ timerIsActive ? 'Стоп' : 'Старт' }}</button>
      </div>

      <ul class="timers-list">
        <li class="timers-list-item"
          v-for="(item, index) in timersList" :key="index">
          <input type="text" v-model="item.descr">
          <button>Сохранить</button>
          <p class="timers-list-time">Продолжительность таймера - <span>{{ item.time }}</span></p>
          <p class="timers-list-start">Время старта таймера - <span>{{ item.start }}</span></p>
          <p class="timers-list-end">Время конца таймера - <span>{{ item.end }}</span></p>
          <button  class="delete-timer" @click="deleteTimer(index)">Удалить таймер</button>
        </li>
      </ul>
    </div>
  </section>
</template>

<script>

export default {
  data() {
    return {
      appTime: {
        appHours: 0,
        appMinutes: 0,
        appSeconds: 0
      },
      timerInterval: new Function(),
      cuurentTimerDescr: '',
      timerIsActive: false,
      startTime: '',
      timersList: []
    }
  },

  computed: {
    currentTime() {
      let currentHours = this.formatTimeNum(this.appTime.appHours);
      let currentMinutes = this.formatTimeNum(this.appTime.appMinutes);
      let currentSeconds = this.formatTimeNum(this.appTime.appSeconds);
      return currentHours + ':' + currentMinutes + ':' + currentSeconds;
    }
  },



  methods: {
    formatTimeNum(num) {
      return (num < 10) ? '0' + num : num;
    },



    pushTimer() {
      const nowTimeIs  = new Date();
      const timerObj = {
        descr: this.cuurentTimerDescr == '' ? 'Нет описания' : this.cuurentTimerDescr,
        time: this.currentTime,
        start: this.startTime,
        end: this.formatTimeNum(nowTimeIs.getHours()) + ':' + this.formatTimeNum(nowTimeIs.getMinutes()) + ':' + this.formatTimeNum(nowTimeIs.getSeconds())
      };
      this.timersList.push(timerObj);
    },

    deleteTimer(i) {
      this.timersList.splice(i, 1);
    },

    timerTick() {
      if(this.appTime.appSeconds < 59) {
        this.appTime.appSeconds++;
      } else {
        this.appTime.appSeconds = 0;
        this.appTime.appMinutes++;
        if(this.appTime.appMinutes >= 60) {
          this.appTime.appMinutes = 0;
          this.appTime.appHours++;
        }
      }
    },



    startStopTimer() {
      if(this.timerIsActive == false) {
        const nowTimeIs = new Date();
        this.timerIsActive = !this.timerIsActive;
        this.startTime = this.formatTimeNum(nowTimeIs.getHours()) + ':' + this.formatTimeNum(nowTimeIs.getMinutes()) + ':' + this.formatTimeNum(nowTimeIs.getSeconds());
        this.timerInterval = setInterval(() => {
          this.timerTick();
        }, 1000);
      } else {
        this.timerIsActive = !this.timerIsActive;
        this.pushTimer();
        clearInterval(this.timerInterval);
        this.cuurentTimerDescr = '';
        this.appTime.appHours = 0;
        this.appTime.appMinutes = 0;
        this.appTime.appSeconds = 0;
      }
    }
  }
}
</script>

<style>
  body {
    margin: 0;
  }
  body * {
    box-sizing: border-box;
    font-family: sans-serif;
    color: rgb(80, 80, 80);
  }
  input[type="text"] {
    width: 100%;
    border-radius: 5px;
    border: 1px solid rgb(214, 214, 214);
    padding: 8px;
    font-size: 14px;
    margin-bottom: 8px;
  }

  button {
      padding-top: 8px;
      padding-bottom: 8px;
      border: none;
      border-radius: 5px;
      background-color: #fff;
      background: rgb(110, 141, 228);
      color: #fff;
      cursor: pointer;
    }
    .delete-timer {
      background-color: rgb(189, 90, 52);
    }
  .time-app-cont {
    width: 100vw;
    height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .time-app {
    position: relative;
    width: 400px;
    padding: 15px;
    background-color: rgb(255, 255, 255);
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    border-radius: 5px;
    
  }
  input+button {
    display: none;
    margin-bottom: 8px;
  } 
  input:focus+button {
    display: inline-block;
  }
  .current-time {
    border-radius: 5px;
    background-color: rgb(255, 255, 255);
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 48px;
    font-weight: 500;
    padding-top: 16px;
    padding-bottom: 8px;
  }
  .current-time p {
    margin: 0;
    line-height: 1;
  }
  .start-stop-btn {
    margin: auto;
    display: block;
    font-size: 16px;
    padding: 0;
    width: 100px;
    height: 100px;
    border-radius: 50%;
    border: none;
    background-color: rgb(91, 189, 120);
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    color: #fff;
    cursor: pointer;
  }
  .start-stop-btn.is-active {
    background-color: rgb(189, 90, 52);
  }
  .start-stop-btn:active {
    transform: scale(0.95);
  }



  .timers-list {
    padding: 0;
    list-style-type: none;
    max-height: 300px;
    overflow-y: scroll;
  }
  .timers-list-item {
    border-top: 1px solid rgb(214, 214, 214);
    padding-top: 16px;
    padding-bottom: 16px;
  }
  .timers-list-item p {
    margin: 0;
    margin-bottom: 8px;
  }

  .timers-list-item p span {
    color: rgb(63, 139, 189);
    display: inline-block;
  }

  .timers-list-title {
    margin-top: 0;
    margin-bottom: 8px;
  }
</style>

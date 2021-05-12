<template>
  <div class="demo">
    <TimeBlock v-for="item in timerArr" :info="item" :key="item.name" />
  </div>
</template>

<script>
import TimeBlock from "./chid-comps/timer-block";

export default {
  props: {
    deadline: {
      type: Number,
    },
  },
  components: {
    TimeBlock,
  },
  data() {
    return {
      timerArr: [
        {
          name: "Days",
          time: null
        },
        {
          name: "Hours",
          time: null
        },
        {
          name: "Minutes",
          time: null
        },
        {
          name: "Seconds",
          time: null
        },
      ],
      timer: null,
    };
  },
  created() {
    this.initClock(this.deadline);
  },
  beforeDestroy() {
    clearInterval(this.timer);
  },
  methods: {
    initClock(endtime) {
      const currentTime = this.getTimeRemaining(new Date(endtime));
      this.updateClock(endtime);
      // 定時器開始
      this.timer = setInterval(() => this.updateClock(endtime), 1000);
      if (currentTime.total <= 0) {
        clearInterval(this.timer);
      }
    },
    getTimeRemaining(endtime) {
      const total = Date.parse(endtime) - Date.parse(new Date());
      const seconds = Math.floor((total / 1000) % 60);
      const minutes = Math.floor((total / 1000 / 60) % 60);
      const hours = Math.floor((total / (1000 * 60 * 60)) % 24);
      const days = Math.floor(total / (1000 * 60 * 60 * 24));

      return {
        total,
        days,
        hours,
        minutes,
        seconds,
      };
    },
    updateClock(endtime) {
      const currentTime = this.getTimeRemaining(new Date(endtime));
      console.log(currentTime)
      for(let item of this.timerArr) {
        item.time = ("0" + currentTime[item.name.toLowerCase()]).slice(-2)
      }
    },
  },
};
</script>

<style scoped>
.demo {
  margin: 0 auto;
  max-width: 800px;
  height: 100%;
  display: flex;
  justify-content: space-around;
  align-items: center;
}

@media (max-width: 525px) {
  .demo {
    flex-direction: column;
  }
}
</style>
<template>
  <div class="flip-box">
    <div :class="goflip ? 'flip down go' : 'flip down'">
      <!-- 當前值 -->
      <div class="digital front">
        <div class="before">{{ frontValue }}</div>
        <div class="after">{{ frontValue }}</div>
      </div>
      <!-- 下一值 -->
      <div class="digital back">
        <div class="before">{{ time }}</div>
        <div class="after">{{ time }}</div>
      </div>
    </div>
    <div class="bottom-value">{{ info.name }}</div>
  </div>
</template>

<script>
export default {
  props: {
    info: {
      default: Object,
    },
  },
  data() {
    return {
      goflip: true,
      frontValue: null,
    };
  },
  computed: {
    time() {
      return this.info.time;
    },
  },
  watch: {
    time: {
      handler() {
        console.log(this.frontValue, this.time);
        if (this.frontValue !== this.time) {
          this.goflip = true;
          setTimeout(() => {
            this.frontValue = this.time;
            this.goflip = false;
          }, 700);
        }
      },
      immediate: true,
    },
  },
};
</script>

<style scoped>
.flip-box {
  width: 120px;
  padding-top: 10px;
  background-color: rgb(82, 82, 82);
  display: flex;
  align-items: center;
  flex-direction: column;
  border-radius: 10px;
}

.bottom-value {
  padding: 5px 0;
  color: #fff;
}

.flip {
  display: inline-block;
  position: relative;
  width: 100px;
  height: 100px;
  line-height: 100px;
  border-radius: 10px;
  background: #fff;
  font-size: 66px;
  color: rgb(250, 250, 250);
  box-shadow: 0 0 6px rgba(0, 0, 0, 0.5);
  text-align: center;
  font-family: "Helvetica Neue";
}

.flip .digital > .before,
.flip .digital > .after {
  content: "";
  position: absolute;
  left: 0;
  right: 0;
  background: rgb(80 158 91);
  overflow: hidden;
  box-sizing: border-box;
}

.flip .digital > .before {
  top: 0;
  bottom: 50%;
  border-radius: 10px 10px 0 0;
  border-bottom: solid 1px #666;
}

.flip .digital > .after {
  top: 50%;
  bottom: 0;
  border-radius: 0 0 10px 10px;
  line-height: 0;
}

/*向下翻*/
.flip.down .front > .before {
  z-index: 3;
}

.flip.down .back > .after {
  z-index: 2;
  transform-origin: 50% 0%;
  transform: perspective(160px) rotateX(180deg);
}

.flip.down .front > .after,
.flip.down .back > .before {
  z-index: 1;
}

.flip.down.go .front > .before {
  transform-origin: 50% 100%;
  animation: frontFlipDown 0.8s ease-in-out both;
  box-shadow: 0 -2px 6px rgba(255, 255, 255, 0.3);
  backface-visibility: hidden;
}

.flip.down.go .back > .after {
  animation: backFlipDown 0.8s ease-in-out both;
}

@keyframes frontFlipDown {
  0% {
    transform: perspective(160px) rotateX(0deg);
  }

  100% {
    transform: perspective(160px) rotateX(-180deg);
  }
}

@keyframes backFlipDown {
  0% {
    transform: perspective(160px) rotateX(180deg);
  }

  100% {
    transform: perspective(160px) rotateX(0deg);
  }
}
</style>
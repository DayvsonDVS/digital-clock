<template>
  <div id="time">
    <div class="circle" style="--clr: #ff2972">
      <div class="dots hr_dot"></div>
      <svg>
        <circle cx="70" cy="70" r="70"></circle>
        <circle cx="70" cy="70" r="70" id="hh"></circle>
      </svg>

      <div ref="hours"></div>
    </div>
    <div class="circle" style="--clr: #fee800">
      <div class="dots min_dot"></div>
      <svg>
        <circle cx="70" cy="70" r="70"></circle>
        <circle cx="70" cy="70" r="70" id="mm"></circle>
      </svg>

      <div ref="minutes">00</div>
    </div>
    <div class="circle" style="--clr: #04fc43">
      <div class="dots sec_dot"></div>
      <svg>
        <circle cx="70" cy="70" r="70"></circle>
        <circle cx="70" cy="70" r="70" id="ss"></circle>
      </svg>

      <div ref="seconds">00</div>
    </div>
    <div class="ap">
      <div ref="ampm">AM</div>
    </div>
  </div>
</template>

<script setup lang="ts">
import 'assets/scss/main.scss'

const hours = ref<HTMLElement>()
const minutes = ref<HTMLElement>()
const seconds = ref<HTMLElement>()
const ampm = ref<HTMLElement>()

const list = [
  { time: 'hours', color: '#ff2972', id: 'hh' },
  { time: 'minutes', color: '#fee800', id: 'mm' },
  { time: 'seconds', color: '#04fc43', id: 'ss' },
  { time: 'ampm' }
]

onMounted(() => {
  setInterval(() => {
    let h = new Date().getHours()
    let m = new Date().getMinutes()
    let s = new Date().getSeconds()

    let am = h >= 12 ? 'PM' : 'AM'

    const hh = document.getElementById('hh')
    const mm = document.getElementById('mm')
    const ss = document.getElementById('ss')

    const hr_dot = document.querySelector('.hr_dot')
    const min_dot = document.querySelector('.min_dot')
    const sec_dot = document.querySelector('.sec_dot')

    if (h > 12) {
      h = h - 12
    }

    hours.value!.innerHTML =
      h < 10 ? '0' + h : h.toString() + '<br><span>Hours</span>'
    minutes.value!.innerHTML =
      m < 10 ? '0' + m : m.toString() + '<br><span>Minutes</span>'
    seconds.value!.innerHTML =
      s < 10 ? '0' + s : s.toString() + '<br><span>Seconds</span>'
    ampm.value!.innerHTML = am.toString()

    hh!.style.strokeDashoffset = (440 - (440 * h) / 12).toString()
    mm!.style.strokeDashoffset = (440 - (440 * m) / 60).toString()
    ss!.style.strokeDashoffset = (440 - (440 * s) / 60).toString()

    hr_dot?.setAttribute('style', `transform: rotate(${h * 30}deg)`)
    min_dot?.setAttribute('style', `transform: rotate(${m * 6}deg)`)
    sec_dot?.setAttribute('style', `transform: rotate(${s * 6}deg)`)
  })
})
</script>

<style scoped lang="scss">
#time {
  display: flex;
  gap: 40px;
  color: #fff;
  div {
    position: absolute;
    text-align: center;
    font-weight: 500;
    font-size: 1.5em;
    :nth-child(2) {
      position: absolute;
      transform: translateX(-50%) translateY(-10px);
      font-size: 0.35em;
      font-weight: 300;
      letter-spacing: 0.1em;
      text-transform: uppercase;
    }
  }
  .ap {
    position: relative;
    font: 1em;
    transform: translateX(-20px);
  }
  .circle {
    position: relative;
    width: 170px;
    height: 170px;
    display: flex;
    justify-content: center;
    align-items: center;
    .dots {
      position: absolute;
      width: 100%;
      height: 100%;
      z-index: 10;
      display: flex;
      justify-content: center;
      &::before {
        top: 8px;
        content: '';
        position: absolute;
        width: 15px;
        height: 15px;
        background: var(--clr);
        border-radius: 50%;
        box-shadow: 0 0 20px var(--clr), 0 0 60px var(--clr);
      }
    }
    svg {
      position: relative;
      width: 150px;
      height: 150px;
      transform: rotate(270deg);
      circle {
        display: block;
        width: 100%;
        height: 100%;
        fill: transparent;
        stroke: #191919;
        stroke-width: 4;
        transform: translate(5px, 5px);
        &:nth-child(2) {
          stroke: var(--clr);
          stroke-dasharray: 440;
        }
      }
    }
  }
}
</style>

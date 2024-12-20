<template>
  <div class="eye-hand-coordination-test">
    <h1>Тест на координацию глаз и рук</h1>

    <div v-if="!testStarted">
      <p>Нажмите кнопку, чтобы начать тест.</p>
      <button @click="startTest">Начать тест</button>
    </div>

    <div v-if="testStarted && !testCompleted">
      <p>Раунд {{ round }} из 10</p>
      <div class="game-area">
        <div class="vertical-axis"></div>
        <div class="horizontal-axis"></div>
        <div
          class="circle"
          :style="{ width: circleSize + 'px', height: circleSize + 'px', borderColor: circleColor }"
        ></div>
        <div
          v-for="(ball, index) in balls"
          :key="index"
          class="ball"
          :style="{
            left: index === 0 ? ball.position + '%' : '50%',
            top: index === 1 ? ball.position + '%' : '50%',
          }"
        ></div>
      </div>
      <button @click="stopBall">Остановить шарик</button>
    </div>

    <div v-if="testCompleted">
      <p>Ваш результат: {{ score }} из 10</p>
      <button @click="resetTest">Пройти заново</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      testStarted: false,
      testCompleted: false,
      round: 1,
      score: 0,
      balls: [
        { position: 0, direction: 1, moving: true },
        { position: 0, direction: 1, moving: false },
      ],
      activeBall: 0,
      speed: 1,
      intervalId: null,
      circleSize: 200,
      circleColor: "white",
    };
  },
  methods: {
    startTest() {
      this.testStarted = true;
      this.testCompleted = false;
      this.round = 1;
      this.score = 0;
      this.speed = 1;
      this.circleSize = 200;
      this.circleColor = "white";
      this.nextRound();
    },
    nextRound() {
      this.activeBall = 0;
      this.balls = [
        { position: Math.random() * 100, direction: 1, moving: true },
        { position: 0, direction: 1, moving: false },
      ];
      this.circleSize = Math.max(20, 200 - this.round * 15);
      this.circleColor = "white";
      this.animateBalls();
    },
    animateBalls() {
      clearInterval(this.intervalId);
      this.intervalId = setInterval(() => {
        this.balls.forEach((ball) => {
          if (ball.moving) {
            ball.position += ball.direction * this.speed;
            if (ball.position <= 0 || ball.position >= 100) {
              ball.direction *= -1;
            }
          }
        });
      }, 50);
    },
    stopBall() {
      const ball = this.balls[this.activeBall];
      ball.moving = false;

      const radius = this.circleSize / 2;
      const center = 50;
      const inCircle = (position) => Math.abs(position - center) <= (radius / 400) * 100;

      if (this.activeBall === 1) {
        const firstInCircle = inCircle(this.balls[0].position);
        const secondInCircle = inCircle(this.balls[1].position);

        if (firstInCircle && secondInCircle) {
          this.circleColor = "green";
          this.score++;
        } else if (firstInCircle || secondInCircle) {
          this.circleColor = "yellow";
        } else {
          this.circleColor = "red";
        }

        clearInterval(this.intervalId);
        if (this.round < 10) {
          this.round++;
          this.speed += 0.5;
          setTimeout(() => this.nextRound(), 1000);
        } else {
          this.testCompleted = true;
        }
      } else {
        this.activeBall = 1;
        this.balls[1].moving = true;
      }
    },
    resetTest() {
      this.testStarted = false;
      this.testCompleted = false;
      clearInterval(this.intervalId);
    },
  },
};
</script>

<style>
.eye-hand-coordination-test {
  text-align: center;
  padding: 20px;
}

.game-area {
  position: relative;
  width: 400px;
  height: 400px;
  margin: 20px auto;
  border: 2px solid #ccc;
}

.vertical-axis,
.horizontal-axis {
  position: absolute;
  background: black;
}

.vertical-axis {
  width: 2px;
  height: 100%;
  left: 50%;
  top: 0;
  transform: translateX(-50%);
}

.horizontal-axis {
  height: 2px;
  width: 100%;
  top: 50%;
  left: 0;
  transform: translateY(-50%);
}

.circle {
  position: absolute;
  border: 2px dashed;
  border-radius: 50%;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}

.ball {
  position: absolute;
  width: 20px;
  height: 20px;
  background: blue;
  border-radius: 50%;
  transform: translate(-50%, -50%);
}
</style>

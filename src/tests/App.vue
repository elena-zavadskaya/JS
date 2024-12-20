<template>
  <div class="memory-and-color-test">
    <h1>Тесты</h1>

    <section>
      <memory-test />
    </section>

    <section>
      <color-test />
    </section>

    <section>
      <odd-one-out-test />
    </section>

    <section>
      <color-blindness-test />
    </section>

    <section>
      <eye-hand-coordination-test />
    </section>
  </div>
</template>

<script>
import MemoryTest from "./MemoryTest.vue";
import ColorTest from "./ColorTest.vue";
import OddOneOutTest from "./OddOneOutTest.vue";
import ColorBlindnessTest from "./ColorBlindnessTest.vue";
import EyeHandCoordinationTest from "./EyeHandCoordinationTest.vue";

export default {
  components: {
    MemoryTest,
    ColorTest,
    OddOneOutTest,
    ColorBlindnessTest,
    EyeHandCoordinationTest,
  },
};
</script>

<style>
.memory-and-color-test {
  text-align: center;
  padding: 20px;
  padding-bottom: 200px;
}

section {
  margin: 40px 0;
}
</style>

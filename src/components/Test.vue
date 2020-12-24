<template>
  <div>
    <p>{{ name }}</p>
    <p>{{ count }}</p>
    <br>
    <p>{{ state.a }}</p>
    <p>{{ state.count }}</p>
  </div>
</template>

<script>
import { ref, reactive, computed, readonly, watchEffect, watch, onMounted, onUpdated, onUnmounted } from 'vue';

export default {
  props: {
      name: {
        type: String,
        default: 'name'
      }
  },
  setup(props, context) {
    let count = ref(0);
    const state = reactive({
      a: 'a',
      c: 1,
      count
    })
    const copyState = readonly(state);

    const stopWatch = watchEffect((onInvalidate) => {
      console.log(copyState.count);
      onInvalidate(() => {
      })
    });

    state.count = 123123;
    stopWatch();

    //
    const plusOne = computed({
      get() {
        return count.value +1
      },
      set(value) {
        count.value = value - 1;
      }
    });

    plusOne.value = 1;

    setTimeout(() => {
      count.value ++;
    }, 1000);

    watch(() => state.count, (newVal, oldVal) => {
      console.log(newVal);
    });

    onMounted(() => {
      console.log('mounted!')
    })
    onUpdated(() => {
      console.log('updated!')
    })
    onUnmounted(() => {
      console.log('unmounted!')
    })

    return {
      count,
      state
    }
  }
}
</script>

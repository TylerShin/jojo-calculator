<template>
  <div class="home">
    <Calculator />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import Calculator from '@/components/Calc.vue'; // @ is an alias to /src

@Component({
  components: {
    Calculator,
  },
})
export default class Home extends Vue {}
</script>

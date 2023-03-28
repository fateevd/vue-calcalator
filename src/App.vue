<script lang="ts">
import {defineComponent} from "vue";

export default defineComponent({
  data() {
    return {
      numbers: [0, '=', ',', '+', 1, 2, 3, '-', 4, 5, 6, '/', 7, 8, 9].reverse(),
      str: '',
      str2: '',
      operation: '',
    }
  },
  methods: {
    write(item: string): string {

      if (item === 'c') {
        this.str = '';
        this.str2 = '';
        this.operation = '';
        return item;
      }

      if (item === '->') {
        if (this.str2.length > 0) {
          console.log('here')
          this.str2 = [...this.str2].filter((item, index) => index !== this.str2.length - 1).join("");
        } else if (this.str2.length === 0 && this.operation.length !== 0) {
          this.operation = '';
        } else if (this.str2.length === 0 && this.operation === '' && this.str.length >= 0) {
          this.str = [...this.str].filter((item, index) => index !== this.str.length - 1).join("");
        }
        return item;
      }

      if (['+', '-', '/', '*'].includes(item)) {
        return this.operation = item;
      }
      if (item === '=') {
        const operations: Record<string, number> = {
          "+": Number(this.str) + Number(this.str2),
          '-': Number(this.str) - Number(this.str2),
          '/': Number(this.str) / Number(this.str2),
          '*': Number(this.str) * Number(this.str2),
        };
        this.str = String(operations[this.operation]);
        this.str2 = '';
        this.operation = '';
        return this.str;
      }

      if (this.operation === '') {
        this.str += item;
      } else {
        this.str2 += item;
      }
      return item
    }
  }
});
</script>

<template>
  <input class="input-str" type="text" v-model="str">
  <p> {{ operation }} </p>
  <input class="input-str" type="text" v-model="str2">
  <div class="d-flex">
    <button class="block-number" type="button" @click="write('c')"> C</button>
    <button class="block-number" type="button" @click="write('->')"> -></button>
  </div>
  <div class="block">
    <button class="block-number" @click="write(item)" v-for="item in numbers">
      {{ item }}
    </button>
  </div>

</template>

<style scoped>
.d-flex {
  display: flex;
  align-items: center;
}

.input-str {
  font-size: 18px;
  width: 100%;
  display: flex;
  height: 35px;
  outline: none;
  border: none;
  background: inherit;
}

.block {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-template-row: repeat(6, 1fr);
}

.block-number:nth-child(15) {
  grid-column-start: 1;
  grid-column-end: 3;
  grid-row-start: 0;
  grid-row-end: 5;
}

.block-number {
  width: 100%;
  padding: 40px;
}

</style>

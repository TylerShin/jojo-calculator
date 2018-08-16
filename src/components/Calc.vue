<template>
  <div class="calc-wrapper">
    <h1>삼국지 조조전 온라인 코스트 계산기</h1>
    <ul class="selectedList">
      <li v-for="char in selectedChars" :key="char.id">
        {{ char.name }}
      </li>
    </ul>
    <div class="costResult">
      총 Cost: {{ getTotalCost }}
    </div>

    <input class="searchInput" v-model.trim="searchTerm" />
    <ul class="searchList">
      <li @click="selectChar(char)" class="searchItem" v-for="char in searchedResult" :key="char.id + 'search'">
        <strong>{{ char.name }}</strong>
        <span>병종: {{ char.class }}</span>
        <span>코스트: {{ char.cost }} </span>
      </li>
    </ul>

  </div>
</template>

<script lang="ts">
/// <reference path="../types/character.d.ts" />
import { Component, Prop, Vue } from 'vue-property-decorator';

@Component
export default class Calculator extends Vue {
  private allChars: JoJo.Character[] = require('../assets/characters.json');
  private selectedChars: JoJo.Character[] = [];
  private searchTerm = '';

  selectChar(char: JoJo.Character) {
    if (this.selectedChars.length < 5) {
      this.selectedChars.push(char);
      this.searchTerm = '';
    }
  }

  get searchedResult() {
    if (!!this.searchTerm) {
      return this.allChars.filter(
        char =>
          char.name.includes(this.searchTerm) &&
          !this.selectedChars.includes(char),
      );
    }
    return [];
  }

  get getTotalCost() {
    return this.selectedChars.reduce((accum, current) => {
      return accum + current.cost;
    }, 0);
  }
}
</script>

<style scoped lang="scss">
.calc-wrapper {
  width: 1080px;
  margin: 0 auto;
}

.searchInput {
  display: block;
  width: 100%;
}

.selectedList,
.searchList {
  display: block;
  width: 100%;
  list-style: none;
  margin: 0;
  padding: 0;
}

.searchItem {
  border: 1px solid #eee;
  padding: 5px;
  cursor: pointer;

  span {
    margin-left: 5px;
  }
}

.searchItem + .searchItem {
  border-top: 0;
}

@media (max-width: 1080px) {
  .calc-wrapper {
    width: 100%;
  }
}
</style>

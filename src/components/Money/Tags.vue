<template>
  <div class="tags">
    <div class="new">
      <button @click="createTag">新增标签</button>
    </div>
    <ul class="current">
      <li v-for="tag in dataSource" :key="tag" @click="toggle(tag)"
          :class="{selected:selectedTags.indexOf(tag)>=0}">{{tag}}
      </li>
    </ul>
  </div>

</template>

<script lang="ts">
  import {Component, Prop} from 'vue-property-decorator';
  import Vue from 'vue' ;

  @Component
  export default class Tags extends Vue {
    @Prop() readonly dataSource: string[] | undefined;
    selectedTags: string[] = [];

    toggle(tag: string) {
      const index = this.selectedTags.indexOf(tag);
      if (index >= 0) {
        this.selectedTags.splice(index, 1);
      } else {
        this.selectedTags.push(tag);
      }
      this.$emit('update:selected', this.selectedTags);
      console.log(this.selectedTags);

    }

    createTag() {
      const name = window.prompt('输入一个标签名');
      console.log(name);
      if (name) {
        if (this.dataSource) {
          this.$emit('update:dataSource', [...this.dataSource, name]);
        }
      } else {
        alert('标签名不能为空');
      }
    }
  }


</script>

<style lang="scss" scoped>
  .tags {
    font-size: 14px;
    padding: 16px;
    flex-grow: 1;
    display: flex;
    flex-direction: column-reverse;
    > .current {
      display: flex;
      flex-wrap: wrap;
      > li {
        $bg: #d9d9d9;
        background: $bg;
        $h: 24px;
        height: $h;
        line-height: $h;
        border-radius: $h/2;
        padding: 0 16px;
        margin-right: 12px;
        margin-top: 4px;
        &.selected {
          background: darken($bg, 50%);
          color: white;
        }

      }
    }
    > .new {
      padding-top: 16px;
      button {
        background: transparent;
        border: none;
        color: #999;
        border-bottom: 1px solid;
        padding: 0 4px;
      }
    }
  }

</style>
<style lang="stylus" rel="stylesheet/stylus">
  .cartcontrol
    // 去除盒子的最小高度
    font-size: 0
    .cart-decrease
      display: inline-block
      padding: 6px
      opacity: 1
      &.move-enter, &.move-leave-to
        transform: translateX(36px);
        opacity: 0
        .inner
          transform: rotate(180deg)
      &.move-enter-active, &.move-leave-active
        transition: all .4s linear
        .inner
          transition: all .4s linear
      .inner
        display: inline-block
        line-height: 24px
        font-size: 24px
        color: rgb(0, 160, 220)

    .cart-count
      display: inline-block
      vertical-align: top
      width: 12px
      padding-top: 6px
      line-height: 24px
      text-align: center
      font-size: 10px
      color: rgb(147, 153, 159)
      opacity: 1
      &.fade-enter, &.fade-leave-to
        opacity: 0
      &.fade-enter-active, &.fade-leave-active
        transition: all .4s linear
    .cart-add
      display: inline-block
      padding: 6px
      line-height: 24px
      font-size: 24px
      color: rgb(0, 160, 220)
</style>

<template>
  <div class="cartcontrol">
    <transition name="move">
      <!-- 当没有选择的时候,不显示减号 -->
      <div v-show="this.food.count > 0" class="cart-decrease" @click="decreaseCart">
        <span class="inner icon-remove_circle_outline"></span>
      </div>
    </transition>
    <transition name="fade">
      <div v-show="this.food.count > 0" class="cart-count">{{food.count}}</div>
    </transition>
    <div class="cart-add icon-add_circle" @click="addCart"></div>
  </div>
</template>

<script>
// 第79行使用了Vue实例
  import Vue from 'vue';

  export default {
    // 从父页面中拿到food对象
    props: {
      food: {
        type: Object
      }
    },
    methods: {
      addCart () {
        // console.log(1);
        if (!this.food.count) {
//          this.food.count = 1;
          // 该行代码不仅给food对象添加上了count属性, 而且该属性可以使用vue的双向绑定
          // 使用set方法,将count属性加到food中,并实现双向数据绑定
          Vue.set(this.food, 'count', 1);
        } else {
          this.food.count++;
          // console.log(this.food.count);
        }
      },
      decreaseCart () {
        this.food.count--;
        console.log(this.food.count);
      }
    }

  };
</script>



<style lang="stylus" rel="stylesheet/stylus">
@import '../../common/stylus/mixin.styl';
  .star
    .star-48
      height: 20px
      float: left
      & > li
        float: left
        width: 20px
        height: 20px
        background-size: 20px 20px
        margin-right: 22px
        &:last-child
          margin-right: 0px;
        &.on
          bg-image('star48_on')
        &.half
          bg-image('star48_half')
        &.off
          bg-image('star48_off')
    .star-36
      height: 15px
      float: left
      & > li
        float: left
        width: 15px
        height: 15px
        background-size: 15px 15px
        margin-right: 6px
        &:last-child
          margin-right: 0px;
        &.on
          bg-image('star36_on')
        &.half
          bg-image('star36_half')
        &.off
          bg-image('star36_off')
    .star-24
      height: 10px
      float: left
      & > li
        float: left
        width: 10px
        height: 10px
        background-size: 10px 10px
        margin-right: 3px
        &:last-child
          margin-right: 0px;
        &.on
          bg-image('star24_on')
        &.half
          bg-image('star24_half')
        &.off
          bg-image('star24_off')
</style>

<template>
  <div class="star">
    <ul :class="sizeClass">
      <!--// item 要么是on  half  off-->
      <li v-for="item in itemClasses" :class="item"></li>

    </ul>
  </div>

</template>


<script>
  export default {
    props: {
      size: {
        type: Number
      },
      score: {
        type: Number
      }
    },
    computed: {
      sizeClass () {
        return 'star-' + this.size;
      },
      itemClasses () {
        let arr = [];

        /*
        如果分数是3.2, name arr = ['on', 'on', 'on', 'half', 'off'];
        如果分数是3.6, name arr = ['on', 'on', 'on', 'on', 'off'];
        请实现算法
         */
        let score = parseInt(this.score);
        let hasHalf = (this.score - score) < 0.5;
        for (let i = 0; i < score; i++) {
          arr.push('on');
        }
        if ((this.score - score) && hasHalf) {
          arr.push('half');
        } else {
          arr.push('on');
        }
        while (arr.length < 5) {
          arr.push('off');
        }

        console.log(arr, this.score);
        return arr;
      }
    }
  };
</script>



<style lang="stylus" rel="stylesheet/stylus">
@import '../../common/stylus/mixin.styl';
@import './goods.styl';
</style>

<template>
  <div class="goods">
    <div class="menu-wraper" ref="menuWraper">
      <ul>
        <li v-for="(item, index) in goods"
            @click="_selectMenu(index)"
            class="menu-item"
            :class="{current: currentIndex===index}"
            ref="menuList">
          <div class="text border-1px">
            <span v-show="item.type > 0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
          </div>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper" ref="foodsWrapper">
      <ul>
        <li v-for="item in goods" class="food-list" ref="foodList">
          <h1 class="title">{{item.name}}</h1>
          <ul>
            <li v-for="food in item.foods" class="food-item border-1px">
              <div class="icon">
                <img width="57" height="57" :src="food.icon">
              </div>
              <div class="content">
                <h2 class="name">{{food.name}}</h2>
                <p class="desc">{{food.description}}</p>
                <div class="extra">
                  <span class="count">月售{{food.sellCount}}份</span><span>好评率{{food.rating}}%</span>
                </div>
                <div class="price">
                  <span class="now">￥{{food.price}}</span><span class="old"
                                                                v-show="food.oldPrice">￥{{food.oldPrice}}</span>
                </div>
                <div class="cartcontrol-wrapper">
                  <cartcontrol :food="food"></cartcontrol>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <!-- 拿到配送费deliveryPrice,并传给shopcart -->
    <shopcart :deliveryPrice="seller.deliveryPrice"
              :selectFoods="selectFoods"></shopcart>
              <!-- 拿到选中的商品,并传给购物车 -->
  </div>
</template>

<script>
import BScroll from 'better-scroll';
import cartcontrol from '../cartcontrol/cartcontrol.vue';
import shopcart from '../shopcart/shopcart.vue';
export default {
  // 拿到seller
  props: {
    seller: {
      type: Object
    }
  },
  // 注册组件(自定义指令)
  components: {
    cartcontrol: cartcontrol,
    shopcart: shopcart
  },
  data () {
    return {
      goods: [],
      listHeight: [],
      scrrenY: 0
    };
  },
  // 计算属性会自动监听
  computed: {
    currentIndex () {
      for (let i = 0; i < this.listHeight.length; i++) {
        // 判断当前滚动的高度是否介于两个菜单之间
        if (this.scrrenY >= this.listHeight[i] && this.scrrenY < this.listHeight[i + 1]) {
          // 如果是,获取当前的i
          // 将右侧的菜单进行滚动
          this._followScroll(i);
          // console.log(i);
          return i;
        }
      }
    },
    selectFoods () {
      // 定义一空数组
      // 遍历所有食物, 将food.count不是undefined添加到上面的空数组中
      // 返回一个被选好的食品的数组
      let foods = [];
      this.goods.forEach((good) => {
        good.foods.forEach((food) => {
          if (food.count) {
            // console.log('添加food');
            foods.push(food);
            // console.log(foods);
          }
        });
      });
      return foods;
    }
  },
  methods: {
    _initScroll () {
      // 创建一个左边菜单滚动对象
      this.menuScroll = new BScroll(this.$refs.menuWraper, {
        click: true
      });
      // 创建一个右边食物菜单滚动对象
      this.foodsScroll = new BScroll(this.$refs.foodsWrapper, {
        probeType: 3,
        click: true
      });
    /* eslint-disable no-new */
    // 左边菜单滚动对象
      new BScroll(this.$refs.menuWraper);
      // 右边菜单滚动对象
      this.foodsWrapper = new BScroll(this.$refs.foodsWrapper, {probeType: 3});
      // 添加滚动事件 
      this.foodsWrapper.on('scroll', (opition) => {
        // 获取到随Y轴滚动的高度,实时获取
        this.scrrenY = Math.abs(opition.y);
        // console.log(this.scrrenY);
      });
    },
    // 获取到盒子的高度
    _countHeight () {
      let foodList = this.$refs.foodList;
      // console.log(foodList);
      var height = 0;
      this.listHeight.push(height);
      for (var i = 0; i < foodList.length; i++) {
        height += foodList[i].clientHeight;
        this.listHeight.push(height);
      }
      // console.log(this.listHeight);
    },

    _followScroll (index) {
      let dom = this.$refs.menuList[index];
      this.menuScroll.scrollToElement(dom, 300); 
    },
    // 点击选择左侧菜单
    _selectMenu (index) {
      // 获取当前点击的菜单
      let dom = this.$refs.foodList[index];
      // 将右侧菜单滚动到当前点击的菜单
      this.foodsScroll.scrollToElement(dom, 300);
      // console.log(index);
    }
  },
  // 钩子函数,vue实例被生成后调用这个函数
  created () {
    this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    this.$http.get('api/goods').then((response) => {
      response = response.body;
      if (response.errno === 0) {
        this.goods = response.data;
        // console.log(this.goods);
        this.$nextTick(() => {
          this._initScroll();
          this._countHeight();
        });
      }
    });
  }
};
</script>



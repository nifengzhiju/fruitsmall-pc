<!--  -->
<template>
  <div class="head-nav w" v-if="navKey.length > 0">
    <ul>
      <li
        v-for="(item, index) in navKey"
        :key="index"
        @mouseover="showCurrentNav(index, item)"
        @mouseout="hideNavView"
      >
        {{ item }}
        <i class="el-icon-arrow-right"></i>
      </li>
    </ul>
    <transition name="el-zoom-in-center">
      <home-nav-view
        :currentIndex="currentIndex"
        :currentCategory="category"
        ref="navView"
        @mouseover.native="keepView"
        @mouseout.native="hideNavView"
      ></home-nav-view>
    </transition>
  </div>
</template>

<script>
import HomeNavView from "./HomeNavView";
export default {
  name: "home-nav",
  components: {
    HomeNavView,
  },
  data() {
    return {
      currentIndex: -1,
      currentCategory: "",
      timer: -1,
    };
  },
  props: {
    categoryFruits: {
      type: Object,
      default() {
        return {};
      },
    },
  },
  computed: {
    navKey() {
      return Object.keys(this.categoryFruits);
    },
    category() {
      if (this.currentCategory == "") return [];
      return this.categoryFruits[this.currentCategory];
    },
  },
  methods: {
    showCurrentNav(index, item) {
      this.currentIndex = index;
      this.currentCategory = item;
      window.clearTimeout(this.timer);
      this.$nextTick(() => {
        this.$refs.navView.$el.style.opacity = 1;
      });
    },
    hideNavView() {
      this.timer = setTimeout(() => {
        this.currentIndex = -1;
        this.currentCategory = "";
      }, 300);
      this.$refs.navView.$el.style.opacity = 0;
    },
    keepView() {
      this.$refs.navView.$el.style.opacity = 1;
      window.clearTimeout(this.timer);
    },
  },
};
</script>
<style scoped>
.head-nav {
  position: relative;
  margin-top: 10px;
  box-shadow: 0 10px 15px -13px rgba(0, 0, 0, 0.3);
  z-index: 4;
}
ul {
  box-sizing: border-box;
  display: flex;
  padding: 20px 0;
  flex-direction: column;
  justify-content: space-around;
  position: absolute;
  height: 500px;
  left: 0;
  top: 0;
  background-color: rgba(0, 0, 0, 0.4);
}
ul li {
  position: relative;
  width: 150px;
  padding: 20px 30px;
  font-size: 16px;
  color: #fff;
}
ul li i {
  float: right;
}
ul li:hover {
  cursor: pointer;
  color: var(--font);
}
</style>

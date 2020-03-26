<template>
  <div class="container" ref="container" style=" height: 100vh;
    overflow: hidden;">
    <qianzhi ref="signature" background="#ffffff" lineColor="#000000" lineWidth="2"></qianzhi>
    <div class="buttons">
      <div class="btns">
        <button @click="back">
          返回
        </button>
        <button id="reset" @click="reset">
          清空
        </button>
        <button id="get" @click="getData">
          确认
        </button>
      </div>
      <div class="one hint">* 请 横 屏 签 字</div>
    </div>
  </div>
</template>

<script>
import qianzhi from "./qianzhi";
export default {
  name: "sign-in",
  data() {
    return {
      size: 0, //初始画布内存大小
      screenWidth: 0
    };
  },
  components: {
    qianzhi
  },
  created() {
    // document.body.addEventListener("touchmove", this.handler(event), {
    //   passive: false
    // });
  },
  beforeDestroy() {},
  mounted() {
    this.size = this.$refs["signature"].getFile("signature.png").size;
    this.getChange();
  },
  methods: {
    //阻止默认事件
    handler(e) {
      console.log("进入阻止默认事件", event);
      e.preventDefault();
    },
    back() {
      this.$emit("Myclose");
      this.closeSlide();
    },
    reset() {
      this.$refs["signature"].reset();
    },
    getData() {
      const _this = this;
      let file = this.$refs["signature"].getFile("signature.png");
      const formData = new FormData();
      formData.append("file", file);
    },
    getChange() {
      const _this = this;
      window.onresize = function() {
        _this.screenWidth++;
      };
    },
    closeSlide() {
      console.log("删除touchmove禁用");
      document.removeEventListener("touchmove", this.handler, false);
    }
  },
  watch: {
    screenWidth: function(val) {
      console.log(val);
      this.$refs["signature"].getEquipment();
    }
  }
};
</script>

<style lang="less" scoped>
@rem: 750/10rem;
body {
  height: 100vh;
  overflow: hidden;
}
.container {
  -webkit-overflow-scrolling: touch;
  position: relative;
  @media screen and (orientation: portrait) {
    /* 竖屏情况下 */
    /* portrait-specific styles */
    .buttons {
      .btns {
        position: absolute;
        bottom: 300 / @rem;
        -webkit-transform: rotate(90deg);
        transform: rotate(90deg);
        left: -230 / @rem;
        button {
          height: 64 / @rem;
          width: 182 / @rem;
          font-size: 28 / @rem;
          color: white;
          border-radius: 10 / @rem;
          background-color: #6095ff;
          border: none;
        }
      }
      .hint {
        position: absolute;
        top: 150 / @rem;
        left: -40 / @rem;
        font-size: 28 / @rem;
        color: #ff9500;
        width: 190 / @rem;
        transform: rotate(90deg);
        -ms-transform: rotate(90deg); /* IE 9 */
        -moz-transform: rotate(90deg); /* Firefox */
        -webkit-transform: rotate(90deg); /* Safari 和 Chrome */
        -o-transform: rotate(90deg); /* Opera */
      }
    }
  }
  @media screen and (orientation: landscape) {
    .buttons {
      .btns {
        position: absolute;
        bottom: 30 / @rem;
        right: 20 / @rem;
        button {
          height: 60 / @rem;
          width: 130 / @rem;
          font-size: 18 / @rem;
          color: white;
          border-radius: 10 / @rem;
          background-color: #6095ff;
          border: none;
        }
      }
      .hint {
        position: absolute;
        bottom: 40 / @rem;
        left: 40 / @rem;
        font-size: 16 / @rem;
        color: #ff9500;
        width: 190 / @rem;
      }
    }
  }
}
</style>

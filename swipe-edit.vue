<template>
  <div class="tap-edit" ref="classList">
    <div ref="tap" class="tap">
      <slot></slot>
    </div>
    <div ref="edit" class="edit">
      <slot name="edit"></slot>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      touch: {}
    };
  },
  mounted() {
    this.$refs.tap.style.right = 0;
    this.$refs.edit.style.right = `-${this.$refs.edit.offsetWidth}px`;

    this.$refs.tap.addEventListener("touchstart", this.handleTouchStart);
    this.$refs.tap.addEventListener("touchmove", this.handleTouchMove);
    this.$refs.tap.addEventListener("touchend", this.handleTouchEnd);
    this.$refs.tap.addEventListener("mousedown", this.handleTouchStart);
    this.$refs.tap.addEventListener("mousemove", this.handleTouchMove);
    this.$refs.tap.addEventListener("mouseup", this.handleTouchEnd);

    // 防止刚加载时出现删除按钮
    setTimeout(() => {
      this.$refs.classList.setAttribute("class", "tap-edit editing");
    });
  },
  // 钩子
  beforeDestroy () {
    this.$ref.classList.setAttribute("class", "")
  },
  methods: {
    tapDirection(x1, y1, x2, y2) {
      return Math.abs(x1 - x2) > Math.abs(y1 - y2)
        ? x1 - x2 > 0
          ? "Left"
          : "Right"
        : y1 - y2 > 0
        ? "Up"
        : "Down";
    },
    /**
     * 获取当前touch位置
     * @param {object} e 事件绑定函数
     * @returns {object} 坐标
     */
    getTouchPosition(e) {
      let x = 0;
      let y = 0;
      if (e.touches) {
        x = e.touches[0].pageX;
        y = e.touches[0].pageY;
      } else {
        x = e.x || e.pageX;
        y = e.y || e.pageY;
      }
      return { x, y };
    },
    handleTouchStart(e) {
      let { x, y } = this.getTouchPosition(e);
      this.touch.x1 = x;
      this.touch.y1 = y;
    },
    handleTouchMove(e) {
      e.stopPropagation();
      let { x, y } = this.getTouchPosition(e);
      this.touch.x2 = x;
      this.touch.y2 = y;
    },
    handleTouchEnd(e) {
      e.stopPropagation();
      let { x1, y1, x2, y2 } = this.touch;

      if ((x2 && Math.abs(x1 - x2) > 30) || (y2 && Math.abs(y1 - y2) > 30)) {
        this[`handleTap${this.tapDirection(x1, y1, x2, y2)}`]();
      }
      this.touch = {};
    },
    handleTapLeft() {
      // console.log("left");
      this.$emit("tap-swipe");
      this.$refs.tap.style.right = `${this.$refs.edit.offsetWidth}px`;
      this.$refs.edit.style.right = 0;
    },
    handleTapRight() {
      // console.log("right");
      this.$refs.tap.style.right = 0;
      this.$refs.edit.style.right = `-${this.$refs.edit.offsetWidth}px`;
    }
  }
};
</script>

<style>
.tap-edit {
  position: relative;
  overflow: hidden;
}
.editing .tap {
  transition: 0.2s right ease-in-out;
}
.editing .edit {
  transition: 0.2s right ease-in-out;
}
.tap {
  position: relative;
}
.edit {
  position: absolute;
  right: 0;
  top: 0;
  bottom: 0;
}
</style>
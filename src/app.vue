<template>
  <div class="Circleprogress">
    <div class="ring">
      <svg
        :width="width"
        :height="width"
        :viewbox="`0 0 ${width} ${width}`"
        class="svgWrap"
      >
        <circle
          :cx="width * 0.5"
          :cy="width * 0.5"
          :r="width * 0.4"
          :stroke-width="width * 0.05"
          stroke-linecap="round"
          :stroke="backColor"
          fill="none"
          stroke-opacity="0.2"
          :transform="`matrix(0,-1,1,0,0,${width})`"
          :stroke-dasharray="
            `${0.75 * (width * 0.4 * Math.PI * 2)} ${0.25 *
              (width * 0.4 * Math.PI * 2)}`
          "
        ></circle>
        <circle
          ref="progress"
          :cx="width * 0.5"
          :cy="width * 0.5"
          :r="width * 0.4"
          :stroke-width="width * 0.05"
          stroke-linecap="round"
          :stroke="lineColor"
          fill="none"
          :transform="`matrix(0,-1,1,0,0,${width})`"
          :stroke-dasharray="dasharray"
        ></circle>
      </svg>
      <div class="ring_box">
        <div class="fraction">
          <div
            class="desc_"
            :style="{
              color: cssStyle.titleColor
            }"
          >
            {{ title }}
          </div>
          <div
            class="text"
            :style="{
              color: cssStyle.textColor
            }"
          >
            {{ content ? content : (rate * 100) + '%' }}
          </div>
        </div>
        <div
          class="title font-center"
          @click="toComponent"
          :style="{
            color: cssStyle.viewDetailsColor
          }"
        >
          <span>{{ viewDetails }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import {
  defineComponent,
  getCurrentInstance,
  onMounted,
  reactive,
  toRefs
} from "vue";
export default defineComponent({
  name: "Circleprogress",
  props: {
    title: {
      type: String,
      default: "圆环标题"
    },
    content: {
      type: String,
      default: "无穷尽"
    },
    viewDetails: {
      type: String,
      default: "查看一波"
    },
    rate: {
      type: Number,
      default: 1
    },
    width: {
      type: Number,
      default: 100
    },
    lineColor: {
      type: String,
      default: "pink"
    },
    backColor: {
      type: String,
      default: "yellow"
    },
    cssStyle: {
      type: Object,
      default: function() {
        return {
          titleColor: "black",
          textColor: "rgb(88, 218, 88)",
					viewDetailsColor:"pink"
        };
      }
    }
  },
  setup(props, context) {
		// 定义响应式数据
    const state = reactive({
      dasharray: `0 ${props.width * 0.4 * Math.PI * 2}`
    });
		//这里的ctx，类似于vue2中的this 
    const { proxy: ctx } = getCurrentInstance();
		
		// 个人觉着这样写，比较舒适
    const methods = {
      draw(rate) {
        ctx.$nextTick(() => {
          let per = 0.75 * rate;
          let circle = ctx.$refs["progress"];
          let perimeter = circle.getTotalLength(); //圆环的周长
          state.dasharray = perimeter * per + " " + perimeter * (1 - per);
        });
      },
      toComponent() {
				context.emit()
			}
    };
    onMounted(() => {
      ctx.draw(ctx.rate);
    });
    return { ...toRefs(state), ...methods };
  }
});
</script>
<style lang="scss" scoped>
.Circleprogress {
  overflow: hidden;
  .svgWrap {
    transform: rotateZ(-135deg);
    circle {
      /* 圆环的过渡动画 */
      -webkit-transition: stroke-dasharray 800ms;
      transition: stroke-dasharray 800ms;
    }
  }
  .ring {
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    position: relative;
  }
  .ring_box {
    width: 100%;
    height: 100%;
    position: absolute;
    text-align: center;
    top: 0;
    left: 0;
    .fraction {
      font-size: 26px;
      font-weight: bold;
      display: block;
      padding-top: 10%;
      .desc_ {
        font: {
          size: 20px;
          weight: 400;
        }
        color: powderblue;
        text-align: center;
      }
      .text {
        font: {
          size: 42px;
          weight: bold;
        }
        text-align: center;
        padding-top: 16px;
        color: rgb(88, 218, 88);
      }
    }

    .title {
      width: 138px;
      text-align: center;
      border-radius: 22px;
      border: 1px solid rgba(194, 193, 193, 0.8);
      color: blueviolet;
      font: {
        size: 25px;
        weight: 400;
      }
      position: absolute;
      left: calc(50% - 69px);
      bottom: 15px;
    }
  }
}
</style>

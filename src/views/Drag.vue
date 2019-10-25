<template>
  <div>
    <header>元素拖动DEMO</header>
    <div class="drag-box">
      <div class="drage-el-box">
        <div
          class="drag-el"
          draggable="true"
          @dragstart="e=>dragStratHandler(e,'data1')"
          @dragend="e=>dragEndHandler(e,'data1')"
        >元素1</div>
        <div
          class="drag-el"
          draggable="true"
          @dragstart="e=>dragStratHandler(e,'data2')"
          @dragend="e=>dragEndHandler(e,'data2')"
        >元素2</div>
      </div>
      <div>
        <div
          id="data1"
          class="target-el"
          @drop.prevent="e=>dropHandler(e,'data1')"
          @dragover.prevent
        >
          <span>拖放到此处</span>
          <span v-for="(item, index) in dataForm.data1" :key="index">{{index}}</span>
        </div>
        <div
          id="data2"
          class="target-el"
          @drop.prevent="e=>dropHandler(e,'data2')"
          @dragover.prevent
        >
          <span>拖放到此处</span>
          <span v-for="(item, index) in dataForm.data2" :key="index">{{index}}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {},
  components: {},
  data() {
    return {
      dataForm: {
        data1: [],
        data2: []
      }
    };
  },
  computed: {},
  created() {},
  mounted() {},
  watch: {},
  methods: {
    // @dragstart
    dragStratHandler(e, type) {
      let data = {
        type
      };
      let el = document.getElementById(type);
      el.style.borderColor = "#42b983";
      // e.dataTransfer对象是用来进行拖拽元素和目标元素通信的
      e.dataTransfer.setData("data1", JSON.stringify(data));
    },
    // @dragend
    dragEndHandler(e, type) {
      // 将e.dataTransfer对象清除
      e.dataTransfer.clearData();
      let el = document.getElementById(type);
      el.style.borderColor = "#2c3e50";
    },
    // @drop
    dropHandler(e, type) {
      // 获取拖拽元素传来的信息对象
      let data = JSON.parse(e.dataTransfer.getData("data1"));
      if (type == data.type) {
        this.dataForm[type].push(1);
      }
    }
  }
};
</script>

<style scoped lang="scss">
.drage-el-box {
  height: 100px;
  .drag-el {
    width: 100px;
    height: 100px;
    border: 1px solid #eeeeee;
    float: left;
    margin: 0 10px;
    border-radius: 10px;
    line-height: 100px;
    text-align: center;
  }
}
.target-el {
  width: 80%;
  height: 200px;
  position: relative;
  border: 1px dashed #2c3e50;
  border-radius: 10px;
  text-align: left;
  color: #2c3e50;
  margin: 30px auto;
  span:first-child {
    position: absolute;
    color: #e0e0e0;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }
}
</style>

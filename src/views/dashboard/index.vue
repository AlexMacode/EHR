<template>
  <div class="cabinet-wrapper">
    <div class="cabinet-container">
      <div
        v-for="cell in cells"
        :key="cell.id"
        class="cabinet-cell"
        :class="{ 'open': cell.open, 'selected': cell.selected, 'disabled': !cell.isAvaliable }"
        @click="handleCellClick(cell)"
      >
        <div class="cabinet-door">
          <div
            class="cabinet-door-front"
            :style="{ backgroundColor: cell.isAvaliable ? (cell.selected ? 'lightgreen' : 'green') : 'red' }"
          >
            <i v-if="cell.selected" class="el-icon-check"></i>
          </div>
          <div class="cabinet-door-back">
            <div v-if="cell.open" class="cabinet-content">
              <!-- 其他内容可以放在这里 -->
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

  <script>
  import { ElButton, ElTag } from 'element-ui';
  import 'element-ui/lib/theme-chalk/index.css';
  import { mapGetters } from 'vuex'

  export default {
    name: 'Dashboard',
    components: {
      ElButton,
      ElTag
    },
    data() {
      return {
        cells: []
      };
    },
    methods: {
      // 初始化网格数据
      initializeGrid(rows, cols) {
        this.cells = [];
        for (let row = 0; row < rows; row++) {
          for (let col = 0; col < cols; col++) {
            this.cells.push({
              id: `${row}-${col}`,
              open: false,
              selected: false,
              isAvaliable: Math.random() > 0.5, // 随机设置可用性
            });
          }
        }
      },
      // 处理单元格点击事件
      handleCellClick(cell) {
        if (cell.isAvaliable) {
          this.cells.forEach(c => {
            if (c.id === cell.id) {
              c.open = !c.open;
              c.selected = !c.selected;
            } else {
              c.open = false; // 确保只有一个单元格打开
              c.selected = false; // 确保只有一个单元格选中
            }
          });
        }
      },
    },
    created() {
      this.initializeGrid(4, 4); // 初始化 4x4 的网格
    },
  }
</script>

  <style lang="scss" scoped>
  .dashboard {
    &-container {
      margin: 30px;
    }
    &-text {
      font-size: 30px;
      line-height: 46px;
    }
  }
  .cabinet-wrapper {
    display: flex;
    justify-content: center;
    padding: 20px;
    border: 2px solid #409EFF; /* 外部容器的边框 */
    border-radius: 15px; /* 外部容器的圆角 */
    background-color: #f5f5f5; /* 背景颜色 */
  }

  .cabinet-container {
    display: flex;
    flex-wrap: wrap;
    width: 800px; /* 控制柜格容器宽度 */
  }

  .cabinet-cell {
    position: relative;
    width: 100px; /* 单元格宽度 */
    height: 100px; /* 单元格高度 */
    margin: 10px;
    cursor: pointer;
    perspective: 1000px;
    border-radius: 15px; /* 添加圆角 */
    overflow: hidden; /* 防止内容超出边界 */
  }

  .cabinet-cell.disabled {
    cursor: not-allowed; /* 禁用光标 */
    // opacity: 0.5; /* 视觉上禁用 */
  }

  .cabinet-cell.selected {
    border: 2px solid #409EFF; /* 选中效果的边框 */
  }

  .cabinet-door {
    position: absolute;
    width: 100%;
    height: 100%;
    transform-style: preserve-3d;
    transition: transform 0.5s;
    border-radius: 15px; /* 确保柜门也有圆角 */
  }

  .cabinet-door-front, .cabinet-door-back {
    position: absolute;
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: #409EFF;
    color: white;
    font-size: 20px;
    border: 1px solid #ddd;
    box-sizing: border-box;
    backface-visibility: hidden;
    border-radius: 15px; /* 确保柜门的前后都圆角 */
  }

  .cabinet-door-back {
    background-color: #2c3e50;
    transform: rotateY(180deg);
  }

  .cabinet-cell.open .cabinet-door {
    transform: rotateY(180deg);
  }
  .cabinet-icon {
    position: absolute;
    top: 10px;
    left: 10px;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    width: 30px;
    height: 30px;
  }

  .cabinet-content {
    padding: 10px;
    color: #fff;
  }

  .el-button {
    margin-top: 10px;
  }
  </style>

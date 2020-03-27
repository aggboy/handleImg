<template>
  <div style="text-align:center">
    <img @click="startPhoto"
         :src="imgUrl"
         alt="">
    <el-dialog title=""
               :visible.sync="showPhotoDialog"
               :close-on-click-modal='false'
               width="30%"
               center>
      <!-- 滚轮的照片 如果不需要 把他整个img删除即可 -->
      <div class="one-dialog-pic"
           :style="{'width':(style.mywidth+'px'),'height':(style.myheight+'px'),'overflow':'hidden'}">

        <img v-show="style.display"
             :src="imgUrl"
             :style="{'width':'100%;','height':'100%','transform':'scale'+'('+style.scale+')','position':'relative','left':style.left,'top':style.top}">
      </div>

      <!-- 真实的照片 -->
      <img id="wheelImg"
           style="opacity:0; width:100%;height:auto"
           :src="imgUrl"
           @mouseenter="openWheel"
           @mouseleave="closeWheel"
           @mousedown="openDrag">

      <span slot="footer"
            class="dialog-footer">
        <el-button type='danger'
                   @click="openTwoRefuse">取消</el-button>
        <el-button type='primary'
                   @click="handleOneAgree">确认</el-button>
      </span>
    </el-dialog>
  </div>

</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    imgUrl: {
      type: String,
      default: 'https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=4018557288,1217151095&fm=26&gp=0.jpg'
    },
  },
  data () {
    return {
      showPhotoDialog: false,
      style: {
        mywidth: 400,
        myheight: 400,
        display: false,
        scale: 1,
        left: null,
        top: null
      }
    }
  },
  methods: {
    startPhoto () {
      this.showPhotoDialog = true
      this._hanleInitRollImg()
      this._handleInitDrag()
    },
    openWheel () {
      this._initMouseWheel()
    },
    closeWheel () {
      this.stopMouseWheel()
    },
    // 鼠标滚轮操作
    rollImg (e) {
      this._stopScrollFunc()
      if (e.wheelDelta) { // IE/Opera/Chrome
        let t1 = e.wheelDelta
        if (t1 > 0) {
          this.style.scale = this.style.scale + 0.1
        } else {
          this.style.scale = this.style.scale - 0.1
        }
      } else if (e.detail) { // Firefox
        let t2 = e.detail
        if (t2 > 0) {
          this.style.scale = this.style.scale + 0.1
        } else {
          this.style.scale = this.style.scale - 0.1
        }
      }
      return false
    },
    // 鼠标 双击事件 开启拖拽模式
    openDrag (e) {
      let that = this
      let odiv = e.target      // 获取目标元素
      // 阻止默认事件的方法,如果不阻止默认事件onmouseup会无法触发
      e.preventDefault()
      // 算出鼠标相对元素的位置
      let disX = e.clientX - odiv.offsetLeft
      let disY = e.clientY - odiv.offsetTop
      // 计算两边坐标
      document.onmousemove = function (e) {
        // 鼠标按下并移动的事件
        // 用鼠标的位置减去鼠标相对元素的位置，得到元素的位置
        // 因为img居中对齐，所以还要减去目标元素距离body的偏移量
        let left = (e.clientX - disX - odiv.offsetLeft) * that.style.scale
        let top = (e.clientY - disY) * that.style.scale
        // 移动当前元素
        that.style.left = left + 'px'
        that.style.top = top + 'px'
      }
      // 鼠标停止移动时，事件移除
      document.onmouseup = function () {
        document.onmousemove = null
        document.onmouseup = null
      }
    },
    _handleInitDrag () {
      this.style.left = 0
      this.style.top = 0
    },
    _hanleInitRollImg () {
      this.style.display = false
      this.style.scale = 1
      setTimeout(() => {
        let rollImg = document.getElementById('wheelImg')
        this.style.mywidth = rollImg.width - 30
        this.style.myheight = rollImg.height - 30
        this.style.display = true
      }, 500)
    },
    _initMouseWheel () {
      // window.addEventListener('mousewheel', this.rollImg, { passive: false })
      // // firefox
      // window.addEventListener('DOMMouseScroll', this.rollImg, { passive: false })
      document.getElementById('wheelImg').addEventListener('DOMMouseScroll', this.rollImg, { passive: false })
      document.getElementById('wheelImg').addEventListener('mousewheel', this.rollImg, { passive: false })
    },
    stopMouseWheel () {
      document.getElementById('wheelImg').removeEventListener('DOMMouseScroll', this.rollImg)
      document.getElementById('wheelImg').removeEventListener('mousewheel', this.rollImg)
    },
    _stopScrollFunc (evt) {
      evt = evt || window.event
      if (evt.preventDefault) {
        // Firefox
        evt.preventDefault()
        evt.stopPropagation()
      } else {
        // IE
        evt.cancelBubble = true
        evt.returnValue = false
      }
      return false
    },
    openTwoRefuse () {
      this.showPhotoDialog = true
    },
    handleOneAgree () {
      this.showPhotoDialog = true
    }
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .one-dialog-pic {
    width: 100%;
    height: 100%;
    position: absolute;
    left: 50%;
    transform: translate(-50%);
  }
</style>

<template>
  <div>
    <!-- 弹出层的头部区域 -->
    <van-nav-bar title="频道管理" class="pop-header">
      <template #right>
        <van-icon name="cross" size="0.37333334rem" color="white" @click="closeFn"/>
      </template>
    </van-nav-bar>
    <!-- 我的频道 -->
    <div class="my-channel-box popup-container">
      <div class="channel-title .pop-header ">
        <span>我的频道
          <span class="small-title">
            点击{{ isEdit ? "删除" : "进入" }}频道
          </span>
        </span>
        <span @click="edit" >{{ isEdit ? "完成" : "编辑" }}</span>
      </div>
      <!-- 我的频道列表 -->
      <van-row type="flex" class="more-channel-box">
        <van-col span="6"  v-for="channel in userChannelList" :key="channel.id" @click="removeFn(channel)">
          <div class="channel-item van-hairline--surround" >
            {{channel.name}}
            <!-- 删除的徽标 -->
            <van-badge color="transparent" class="cross-badge"
            v-show='isEdit'
            v-if="isEdit && channel.name !== '推荐'"
            >
              <template #content>
                <van-icon
                  name="cross"
                  class="badge-icon"
                  color="#cfcfcf"
                  size="0.32rem"
                />
              </template>
            </van-badge>
          </div>
        </van-col>
      </van-row>
    </div>

    <!-- 更多频道 -->
    <div class="more-channel-box">
      <div class="channel-title">
        <span>点击添加更多频道：</span>
      </div>
      <!-- 更多频道列表 -->
      <van-row type="flex">
        <van-col span="6"
         v-for="channel in unCheckChannelList" :key="channel.id"
         @click="addFn(channel)"
        >
          <div class="channel-item van-hairline--surround"> {{channel.name}}
</div>
        </van-col>
      </van-row>
    </div>
  </div>
</template>

<script>
export default {
  props:['userChannelList','unCheckChannelList'],
  data() {
    return {
      isEdit:false
    }
  },
  methods: {
    edit(){
      this.isEdit=!this.isEdit
    },
    addFn(channel){
    if (this.isEdit===true) {
      
      this.$emit('addChannel',channel)
     }else{

     }
    },
    removeFn(obj){
     if (this.isEdit === true) {
    // 处于编辑状态 -> 执行删除功能
    if (obj.id!==0) {
        this.$emit('removeChannel', obj)
    }
    } else{
       this.$emit('closeEV')
        this.$emit('input', obj.id) // 切换

    }

    },
     // 关闭弹出层方法
    closeFn () {
      this.$emit('closeEV')
      // this.isEdit = false
    }
  },
    
}
</script>

<style scoped lang="less">
.van-popup,
.popup-container {
  background-color: transparent;
  height: 100%;
  width: 100%;
}

.popup-container {
  display: flex;
  flex-direction: column;
}

.pop-header {
  height: 90px;
  background-color: #007bff;
  color: white;
  text-align: center;
  font-size: 14px;
  position: relative;
  .title {
    width: 100%;
    position: absolute;
    bottom: 15px;
  }
}

.pop-body {
  flex: 1;
  overflow: scroll;
  padding: 8px;
  background-color: white;
}

.my-channel-box,
.more-channel-box {
  .channel-title {
    display: flex;
    justify-content: space-between;
    font-size: 14px;
    line-height: 28px;
    padding: 0 6px;
  }
}

.channel-item {
  font-size: 12px;
  text-align: center;
  line-height: 36px;
  background-color: #fafafa;
  margin: 5px;
}

/*删除的微标 */
.cross-badge {
  position: absolute;
  right: -3px;
  top: 0;
  border: none;
}

/*提示文字 */
.small-title {
  font-size: 10px;
  color: gray;
}
</style>

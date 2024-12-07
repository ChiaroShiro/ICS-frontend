//dashboard里面那个框，右边那个
<template>
  <el-card :body-style="{padding: 0, height: '100%'}" class="info-card back" >
   
    <el-row type="flex" class="info-card-container">
      <el-col :span="8" :style="{'background-color': color}" class="height-100">
        <i :class="['info-card-icon', icon]" :style="{'font-size': iconSize}"></i>
      </el-col>
      <el-col :span="16" class="info-card-text">
        <p :style="textStyle">{{value}}</p>
        <p style="font-weight: 300;">{{message}}</p>
      </el-col>
    </el-row>
  </el-card>
</template>

<script>
  export default {
    name: 'inforCard',
    props: {
      value: [String, Number],
      color: String,
      message: String,
      icon: String,
      iconSize: {
        type: String,
        default: '35px'
      },
      countSize: {
        type: String,
        default: '25px'
      },
      countWeight: {
        type: Number,
        default: 700
      }
    },
    computed: {
      textStyle () {
        return {
          'font-size': this.countSize,
          'font-weight': this.countWeight,
          color: this.color
        }
      }
    }
  }
</script>

<style lang="less" scoped>
  @card-height: 90px;
.back
{ background-color: rgba(0, 255, 255, 0.5); 
  transition: background-color 0.3s; 
  position: relative; 
  overflow: hidden; 
}

.back:hover
{ background-color: rgba(0, 255, 255, 0.7); 
}

.back::after{ content: ""; position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%); width: 200%; height: 200%; background-color: rgba(255, 255, 255, 0.3); border-radius: 50%; transition: all 0.5s; pointer-events: none; }

.back:hover::after{ width: 500%; height: 500%; opacity: 0; }

  .info-card {
    display: inline-block;
    margin-right: 10px;
    width: 250px;
    height: @card-height;
    text-align: center;
    vertical-align: middle;

    &-container {
      height: 100%;
      align-items: center;
      justify-content: center;
    }
    &-icon {
      line-height: @card-height;
      color: white;
    }
    &-text {
      p {
        margin: 0;
        .minor-text {
          font-weight: 300;
          margin-top: 2px;
          font-size: 20px;
        }
      }
    }
  }
</style>

<template>
  <div class="pagination">
    <h1> {{startNumAndEndNum }}----当前的页码 {{ pageNo }}</h1>
    <button :disabled="pageNo==1" @click="$emit('getPageNo',pageNo-1)">上一页</button>
    <button v-if="startNumAndEndNum.start > 1" @click="$emit('getPageNo',1)" :class="{active:pageNo==1}">1</button>
    <button v-if="startNumAndEndNum.start > 2">···</button>

    <!-- 中间部分 -->
    <button
      v-for="(page, index) in startNumAndEndNum.end"
      :key="index"
      @click="$emit('getPageNo',page)"
      v-if="page >= startNumAndEndNum.start"
      :class="{active:pageNo==page}"
    >
        <!-- <button v-if="page >= startNumAndEndNum.start">
            {{ page }}
        </button> -->
        {{ page }}

    </button>
    
    <button v-if="startNumAndEndNum.end < totalPage - 1">···</button>
    <button v-if="startNumAndEndNum.end < totalPage" @click="$emit('getPageNo',totalPage)" :class="{active:pageNo==totalPage}"> {{totalPage}}</button>
    <button :disabled="pageNo==totalPage" @click="$emit('getPageNo',pageNo+1)">下一页</button>
    <button style="margin-left: 30px">共 {{total}} 条</button>
  </div>
</template>
  
<script>
export default {
  name: "Pagination",
  props:['pageNo', 'pageSize', 'total', 'continues'],
  computed:{
    // 总共多少页
    totalPage(){
        // 向上取整
        return Math.ceil(this.total / this.pageSize)
    },
    // 计算连续的页码的起始数字与结束数字
    startNumAndEndNum(){
        // 解构赋值
        const {continues, totalPage, pageNo} = this;
        // 先定义两个变量存储起始数字与结束数字
        let start = 0, end = 0;
        // 连续页码数字5【就是至少五页】，如果出现不正常的现象【就是不够五页】
        if(continues > totalPage){
            start = 1;
            end = totalPage;
        }else{
            // 正常现象【连续页码是5】，但是总页码是大于5的
            start = pageNo - parseInt(continues / 2);
            end = pageNo + parseInt(continues / 2);
            // 把出现不正常的现象【start数字出现0|负数】纠正
            if(start < 1 ){
                start = 1;
                end = continues;
            }
            // 把出现不正常的现象【end数字大于总页码】纠正
            if(end > totalPage){
                end = totalPage;
                //前continues页，totalPage为end了
                // 第一个if判断了，所以这个 totalPage - continues 肯定大于0
                start = totalPage - continues + 1;
            }
        }
        return { start, end };
    }
  }
};
</script>
  
<style lang="less" scoped>
.pagination {
    text-align: center;
  button {
    margin: 0 5px;
    background-color: #f4f4f5;
    color: #606266;
    outline: none;
    border-radius: 2px;
    padding: 0 4px;
    vertical-align: top;
    display: inline-block;
    font-size: 13px;
    min-width: 35.5px;
    height: 28px;
    line-height: 28px;
    cursor: pointer;
    box-sizing: border-box;
    text-align: center;
    border: 0;

    &[disabled] {
      color: #c0c4cc;
      cursor: not-allowed;
    }

    &.active {
      cursor: not-allowed;
      background-color: #409eff;
      color: #fff;
    }
  }
}

</style>
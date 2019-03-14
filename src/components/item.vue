<!--<template>-->
  <!--<div class="list-item editingClass editing"> &lt;!&ndash; 最外层容器&ndash;&gt;-->
    <!--<label class="checkbox"> &lt;!&ndash;自定义的多选框&ndash;&gt;-->
      <!--<input type="checkbox" v-model="item.checked"> &lt;!&ndash;item.checked&ndash;&gt;-->
      <!--<span class="checkbox-custom"></span>-->
    <!--</label>-->
    <!--<input type="text" v-model="item.text" placeholder='写点什么。。。'>  &lt;!&ndash;绑定item.text&ndash;&gt;-->
    <!--<a class="delete-item"> &lt;!&ndash;删除图标&ndash;&gt;-->
      <!--<span class="icon-trash"></span>-->
    <!--</a>-->
  <!--</div>-->
<!--</template>>-->


<!--<script>-->
  <!--export default {-->
    <!--props: ['item']  //子组件显式的用 props 选项声明它期待获得的数据，-->
    <!--// 这里申明 它想要一个叫做 ’item‘的数据。-->
  <!--};-->

<!--</script>-->
<!--<style lang="less">-->
  <!--@import '../common/style/list-items.less';-->
<!--</style>-->



<template>
  <div class="list-item editingClass editing">
    <label class="checkbox">
      <input type="checkbox" v-model="item.checked">
      <span class="checkbox-custom"></span>
    </label>
    <input type="text" v-model="item.text" placeholder='写点什么。。。'>
    <a class="delete-item">
      <span class="icon-trash"></span>
    </a>
  </div>
</template>
<script>
  // item 是todo的子组件,他接受一个对象item,来进行处理
  export default {
    props: {
      item: {
        type: Object,
        default: () => {
          return {
            checked: false,
            text: '你好,世界'
          }
        }
      }
    }
  };
</script>
<style lang="less">
  @import '../common/style/list-items.less';
</style>

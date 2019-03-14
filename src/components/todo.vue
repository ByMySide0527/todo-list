<!--<template>-->
  <!--<div class="page lists-show">&lt;!&ndash;最外层容器&ndash;&gt;-->
    <!--<nav>&lt;!&ndash;容器上半部分&ndash;&gt;-->
      <!--<div class="nav-group"> &lt;!&ndash;移动端的菜单图标&ndash;&gt;-->
        <!--<a class="nav-item">-->
          <!--<span class="icon-list-unordered">-->
          <!--</span>-->
        <!--</a>-->
      <!--</div>-->
      <!--<h1 class="title-page">-->
        <!--<span class="title-wrapper">{{todo.title}}</span> &lt;!&ndash; 标题&ndash;&gt;-->
        <!--<span class="count-list">{{todo.count}}</span>&lt;!&ndash; 数目&ndash;&gt;-->
      <!--</h1>-->
      <!--<div class="nav-group right">&lt;!&ndash; 右边的删除，锁定图标容器&ndash;&gt;-->
        <!--<div class="options-web">-->
          <!--<a class=" nav-item"> &lt;!&ndash; 锁定图标&ndash;&gt;-->
            <!--<span class="icon-lock" v-if="todo.locked"></span>-->
            <!--<span class="icon-unlock" v-else>-->
            <!--</span>-->
          <!--</a>-->
          <!--<a class=" nav-item">&lt;!&ndash; 删除图标&ndash;&gt;-->
            <!--<span class="icon-trash">-->
            <!--</span>-->
          <!--</a>-->
        <!--</div>-->
      <!--</div>-->

      <!--<div class=" form todo-new input-symbol"> &lt;!&ndash; 新增单个代办单项输入框,监听了回车事件，双向绑定text值,监听了disabled属性，在todo.locked为true的情况下无法编辑&ndash;&gt;-->
        <!--<input type="text" v-model="text" placeholder='请输入'@keyup.enter="onAdd" :disabled="todo.locked" />-->
        <!--<span class="icon-add"></span>-->
      <!--</div>-->
    <!--</nav>-->
    <!--<div class="content-scrollable list-items">-->
      <!--&lt;!&ndash;容器下半部分&ndash;&gt;-->
      <!--<div v-for="item in items">-->
        <!--<item :item="item"></item>-->
      <!--</div>-->
    <!--</div>-->
  <!--</div>-->
<!--</template>-->


<!--<script>-->
  <!--import item from './item'-->
  <!--export default {-->
    <!--components :{-->
      <!--item-->
    <!--},-->
    <!--data() {-->
      <!--return {-->
        <!--todo: { //详情内容-->
          <!--title: '星期一',-->
          <!--count: 12,-->
          <!--locked: false-->
        <!--},-->
        <!--items: [ //代办单项列表-->
          <!--{ checked: false, text: '新的一天', isDelete: false },-->
          <!--{ checked: false, text: '新的一天', isDelete: false },-->
          <!--{ checked: false, text: '新的一天', isDelete: false }-->
        <!--],-->
        <!--text: '' //新增代办单项绑定的值-->
      <!--}-->
    <!--},-->
    <!--methods: {-->
      <!--onAdd() {-->
        <!--this.items.push({-->
          <!--checked: false, text: this.text, isDelete: false-->
        <!--}); // 当用户点击回车时候 ，给items的值新增一个对象，this.text 即输入框绑定的值-->
        <!--this.text = ''; //初始化输入框的值。-->
      <!--}-->
    <!--}-->
  <!--}-->
<!--</script>-->
<!--<style lang = "less">-->
  <!--@import '../common/style/nav.less';-->
  <!--@import '../common/style/form.less';-->
  <!--@import '../common/style/todo.less';-->
<!--</style>-->


<template>
  <div class="page lists-show">
    <!-- 头部模块 -->
    <nav>
      <!-- 当用户浏览车窗口尺寸小于40em时候，显示手机端的菜单图标 -->
      <div class="form list-edit-form" v-show="isUpdate">
        <!-- 当用户点击标题进入修改状态，就显示当前内容可以修改 -->
        <input type="text" v-model="todo.title" @keyup.enter="updateTitle" :disabled="todo.locked">
        <div class="nav-group right">
          <a class="nav-item" @click="isUpdate = false">
            <span class="icon-close">
            </span>
          </a>
        </div>
      </div>

      <div class="nav-group" @click="$store.dispatch('updateMenu')" v-show="!isUpdate">
        <a class="nav-item">
          <span class="icon-list-unordered">
          </span>
        </a>
      </div>
      <!-- 显示标题和数字模块 -->
      <h1 class="title-page">
        <span class="title-wrapper">{{todo.title}}</span>  <!-- title:标题 绑定标题 -->
        <span class="count-list">{{todo.count || 0}}</span><!-- count:数量 绑定代办单项熟练-->
      </h1>
      <!-- 右边显示删除图标和锁定图标的模块 -->
      <div class="nav-group right">
        <div class="options-web">
          <a class=" nav-item">
            <!-- icon-lock锁定的图标
            icon-unlock：非锁定的图标
            -->
            <span class="icon-lock" v-if="todo.locked"></span>
            <span class="icon-unlock" v-else>
            </span>
          </a>
          <a class=" nav-item">
            <span class="icon-trash">
            </span>
          </a>
        </div>
      </div>
      <!-- 用户新增代办事项的input模块 -->
      <div class=" form todo-new input-symbol">
        <!-- 绑定disabled值，当todo.locked为绑定的时候，禁止input输入,双向绑定text,和监听input的回车事件@keyup.enter -->
        <input type="text" v-model="text" placeholder='请输入' @keyup.enter="onAdd" :disabled="todo.locked" />
        <span class="icon-add"></span>
      </div>
    </nav>
    <!-- 列表主体模块 -->
    <div class="content-scrollable list-items">
      <div v-for="item in items">
        <item :item="item"></item>
      </div>
    </div>
  </div>
</template>


<script>
  import item from './item';
  import { getTodo , addRecord} from '../api/api';

  export default {
    components:{
      item
    },
    data() {
      return {
        todo: {
          title: '星期一', //标题
          count: 12, //数量
          locked: false //是否绑定
        },
        items: [  //代办单项列表
        ],
        text: '' //用户输入单项项绑定的输入
      }
    },


    watch: {
      '$route.params.id'() {
        // 监听$route.params.id的变化，如果这个id即代表用户点击了其他的待办项需要重新请求数据。
        this.init();
      }
    },
    created() {
      // created生命周期，在实例已经创建完成，页面还没渲染时调用init方法。
      this.init();
    },
    methods: {
      init() {
        // 获取到 $route下params下的id,即我们在menus.vue组件处传入的数据。
        const ID = this.$route.params.id;
        getTodo({ id: ID }).then(res => {

          let { id, title, count, isDelete, locked, record
          } = res.data.todo;
          // 请求成功，拿到res.data.todo;在将record 赋值到代办单项列表，其它数据赋值到todo对象
          this.items = record;
          this.todo = {
            id: id,
            title: title,
            count: count,
            locked: locked,
            isDelete: isDelete
          };
        });
      },
      onAdd() {
        //当用户输入文字，并且回车时调用次方法。
        const ID = this.$route.params.id;
        addRecord({ id: ID, text: this.text }).then(res => {
          this.text = '';
          this.init();
          //请求成功后初始化
        });
      }
    }
  }
</script>
<style lang = "less">
  @import '../common/style/nav.less';
  @import '../common/style/form.less';
  @import '../common/style/todo.less';
</style>

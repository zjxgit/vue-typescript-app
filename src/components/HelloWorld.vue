<template>
  <div class="hello">
    <img src="../assets/logo.png">
    <h1>{{ msg }}</h1>
    <h2>vue-typescrip-template</h2>
    <p>mixin 数据 ：{{ testMixinArg }}</p>
    <p>store 数据 ：{{ info.data }}</p>
    <RC></RC>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import Component from 'vue-class-component'
import { Prop, Watch, Emit } from 'vue-property-decorator'
import RC from '@/components/renderComponent.vue'
import TestMixin from '../mixins/test-mixin'
import { Getter } from 'vuex-class'

@Component({
  components: {
    RC
  },
  mixins: [TestMixin]
})
export default class HelloWorld extends Vue<TestMixin> {
  @Getter info

  msg: string = 'Welcome to Your Vue-Typescript App'

  // prop声明  相当于Vue中的props继承变量
  // @Prop() public msg: string;
  // @Prop({ default: 'default value' }) propB: string
  // @Prop([String, Boolean]) propC: string | boolean

  created () {
    console.log('created')
  }

  mounted () {
    console.log(this.testMixinArg)
  }

  // 等同于vue watch
  @Watch('msg', { immediate: true, deep: false })
  public onChildChanged(val: string, oldVal: string) {
    console.log('watch new name=' + val)
  }

  // 等同于vue的computed钩子函数 计算属性
  // newMsg是计算后的值，msg是被监听的值
  public get newMsg() {
    return 'computed ' + this.msg
  }

  // method 等同于vue里methods钩子函数里的function
  public clickFunc(): void {
    console.log(this.msg)
    this.changeMsg()
  }

  // Emit 事件传递的两种写法
  // @Emit()
  // addToCount(n: number) {
  //     this.count += n
  // }

  @Emit('reset')
  changeMsg() {
    this.msg = 'emit msg'
    console.log('~~~~~~', this.msg)
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>

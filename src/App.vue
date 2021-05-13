<template>
  <div id="app">
    <div class="container">
      <div class="title">ToDoList</div>
      <input type="text" v-model='todo'  @keydown="doAdd($event)" />
    </div>

    <div class="playing">
      <div>正在进行</div>
    </div>
    <ul class="newadd" id="newadd">
      <li v-for="(item,key) in list" v-if="!item.checked">
          <input type="checkbox" v-model="item.checked"  @change="changeList()" /> {{item.title}} 
          <button title="点击删除"  @click="removeData(key)">-删除-</button>
      </li>
    </ul>
    
    <div class="playing">
      <div>已完成</div>
    </div>
    <ul class="ok">      
      <li v-for="(item,key) in list" v-if="item.checked">
        <input type="checkbox"  v-model="item.checked" />  {{item.title}}
        <button title="点击删除"  @click="removeData(key)">-删除-</button>
      </li>
    </ul>
  </div>
</template>
<script>
import storage from './storage.js'
export default {
  data () { 
        return {
          waitSize:[],
          completeSize:[],
          todo:'' ,
          list: []
        }
      },
      methods:{
        doAdd(e){
          if(this.todo!=''){     //输入框不为空
            if(e.keyCode==13){   //按下回车键
              this.list.push({   //数组添加元素
                title:this.todo, //title为输入的内容
                checked:false    //默认未完成
              })
              this.todo='';
              this.waitSize.push({
                title:this.todo,
                checked:false
              });
            }
          }
          storage.setItem('list',this.list);
        },
        removeData(key){
          this.list.splice(key,1);  //删除数据
          storage.setItem('list',this.list);  //重新保存最新数据
        }, 
        changeList(){
          storage.setItem('list',this.list);
          var list=storage.getItem('list');
          if(list){  /*注意判断*/
            this.list=list;
          }
        }
      },
      mounted(){
        var list=storage.getItem('list');
        if(list){  /*注意判断*/
          this.list=list;
        }
      }
}
</script>>
<style>
@import "style.css";
</style>

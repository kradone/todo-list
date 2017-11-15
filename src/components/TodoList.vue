<template>
  <div id="todolist">
      <el-row gutter="10">
          <el-col :xs="18" :sm="18" :md="22" :lg="22" :xl="22">
            <el-input v-model="input" @keyup.enter.native="addNew" placeholder="添加待办事项"></el-input>
          </el-col>
          <el-col :xs="6" :sm="6" :md="2" :lg="2" :xl="2">
            <el-button v-on:click="addNew" type="primary">添加</el-button>
          </el-col>
      </el-row>
      <el-row>
          <el-col span="24">
            <el-card>
              <el-collapse v-model="activeNames" @change="handlerChange">
                <el-collapse-item title="待完成事项" name="1">
                  <el-card :body-style="{padding:'10px'}">
                    <ul>
                      <li v-for="(item,index) in items" v-bind:key="item.id" v-show="item.isFinished == false">
                        <el-row>
                          <el-col :xs="2" :sm="2" :md="1" :lg="1" :xl="1">
                            <el-checkbox v-model="item.isFinished"></el-checkbox>
                          </el-col>
                          <el-col :xs="18" :sm="18" :md="21" :lg="21" :xl="21" class="todoListText" v-bind:class="{'finished':item.isFinished}">
                            <span class="important hidden-sm-and-down" v-if="item.isImportant == true">重要</span>
                            <span class="urgent hidden-sm-and-down" v-if="item.isUrgent == true">紧急</span>
                            {{item.label}}
                            <span class="time hidden-sm-and-down" v-if="item.todoDate != ''">
                              <i class="el-icon-date"></i>
                              {{item.todoDate}}
                              </span>
                          </el-col>
                          <el-col :md="1" :lg="1" :xl="1" class="hidden-sm-and-down">
                            <el-dropdown :hide-on-click="false" trigger="click">
                              <el-button type="primary" icon="el-icon-edit" size="mini"></el-button>
                              <el-dropdown-menu slot="dropdown">
                                <el-dropdown-item>
                                  <el-input v-model="item.label"></el-input>
                                </el-dropdown-item>
                                <div class="line"></div>
                                <el-dropdown-item>
                                  优先度： 
                                  <el-button size="mini" v-if="item.isImportant == false" v-on:click="toImportant(item)">重要</el-button>
                                  <el-button type="primary" size="mini" v-if="item.isImportant == true" v-on:click="toImportant(item)">重要</el-button>
                                  <el-button size="mini" v-if="item.isUrgent == false" v-on:click="toUrgent(item)">紧急</el-button>
                                  <el-button type="warning" size="mini" v-if="item.isUrgent == true" v-on:click="toUrgent(item)">紧急</el-button>
                                </el-dropdown-item>
                                <div class="line"></div>
                                <el-dropdown-item>
                                  日期：
                                <el-date-picker v-model="item.todoDate" type="date" format="yyyy 年 MM 月 dd 日" value-format="yyyy-MM-dd" placeholder="请输入时间"></el-date-picker>
                                </el-dropdown-item>
                              </el-dropdown-menu>
                            </el-dropdown>
                          </el-col>
                          <el-col :xs="4" :sm="4"  :md="1" :lg="1" :xl="1">
                            <el-button type="danger" icon="el-icon-delete" size="mini" v-on:click="toDelete(item)"></el-button>
                          </el-col>
                        </el-row>
                        <div class="line"></div>
                      </li>
                    </ul>
                  </el-card>               
                </el-collapse-item>
                <el-collapse-item title="已完成事项" name="2">
                  <el-card :body-style="{padding:'10px'}">
                    <ul>
                      <li v-for="(item,index) in items" v-bind:key="item.id" v-show="item.isFinished == true">
                         <el-row>
                          <el-col :xs="2" :sm="2" :md="1" :lg="1" :xl="1">
                            <el-checkbox v-model="item.isFinished"></el-checkbox>
                          </el-col>
                          <el-col :xs="22" :sm="22" :md="21" :lg="21" :xl="21" class="todoListText" v-bind:class="{'finished':item.isFinished}">
                            <span class="important hidden-sm-and-down" v-if="item.isImportant == true">重要</span>
                            <span class="urgent hidden-sm-and-down" v-if="item.isUrgent == true">紧急</span>
                            {{item.label}}
                            <span class="time hidden-sm-and-down" v-if="item.todoDate != ''">{{item.todoDate}}</span>
                          </el-col>
                          <el-col :md="1" :lg="1" :xl="1" class="hidden-sm-and-down">
                            <el-dropdown :hide-on-click="false" trigger="click">
                              <el-button type="primary" icon="el-icon-edit" size="mini"></el-button>
                              <el-dropdown-menu slot="dropdown">
                                <el-dropdown-item>
                                  <el-input v-model="item.label"></el-input>
                                </el-dropdown-item>
                                <div class="line"></div>
                                <el-dropdown-item>
                                  优先度： 
                                  <el-button size="mini" v-if="item.isImportant == false" v-on:click="toImportant(item)">重要</el-button>
                                  <el-button type="primary" size="mini" v-if="item.isImportant == true" v-on:click="toImportant(item)">重要</el-button>
                                  <el-button size="mini" v-if="item.isUrgent == false" v-on:click="toUrgent(item)">紧急</el-button>
                                  <el-button type="warning" size="mini" v-if="item.isUrgent == true" v-on:click="toUrgent(item)">紧急</el-button>
                                </el-dropdown-item>
                                <div class="line"></div>
                                <el-dropdown-item>
                                  日期：
                                <el-date-picker v-model="item.todoDate" type="date" format="yyyy 年 MM 月 dd 日" value-format="yyyy-MM-dd" placeholder="请输入时间"></el-date-picker>
                                </el-dropdown-item>
                              </el-dropdown-menu>
                            </el-dropdown>
                          </el-col>
                          <el-col :md="1" :lg="1" :xl="1" class="hidden-sm-and-down">
                            <el-button type="danger" icon="el-icon-delete" size="mini" v-on:click="toDelete(item)"></el-button>
                          </el-col>
                        </el-row>
                        <div class="line"></div>
                      </li>
                    </ul>
                  </el-card>               
                </el-collapse-item>
              </el-collapse>
            </el-card>
          </el-col>
      </el-row>
  </div>
</template>

<script>
import Store from "../store"
export default {

  name: "TodoList",
  data() {
    return {
      input: "",
      items: Store.fetch() == null ? [] : Store.fetch(),
      activeNames: ["1"]
    };
  },
  watch: {
    items: {
      handler(val, oldVal) {
        Store.save(this.items);
      },
      deep: true
    }
  },
  methods: {
    addNew() {
      this.items.push({
        label: this.input,
        isFinished: false,
        isImportant:false,
        isUrgent:false,
        todoDate:''
      });
      this.input = "";
    },
    toDelete(index) {
      this.items.splice(index,1);
    },
    toImportant(item) {
      item.isImportant = !item.isImportant;
    },
    toUrgent(item) {
      item.isUrgent = !item.isUrgent;
    }
  }
};
</script>

<style scoped>
.el-row {
  padding: 10px;
}
#todolist {
  background-color: #edf2fc;
}
.finished {
  color:#B4BCCC;
  text-decoration: line-through;
}
ul {
  padding-left: 0px;
}
li {
  display: block;
}
.todoListText {
  font-size: 16px;
}
.line{
  height: 1px;
  width: 100%;
  margin-left: auto;
  margin-right: auto;
  background-color:#D8DCE5;
  margin-top: 5px;
  margin-bottom: 5px;
}
.important{
  color: #409EFF;
  border: solid 1px;
  font-size: 12px;
  padding: 2px;
  border-radius: 2px;
  margin-left: 2px;
  margin-right: 2px;
}
.urgent{
  color: #EB9E05;
  border: solid 1px;
  font-size: 12px;
  padding: 2px;
  border-radius: 2px;
  margin-left: 2px;
  margin-right: 2px;
}
.time{
  color: #878D99;
  border: solid 1px;
  font-size: 14px;
  padding: 2px;
  border-radius: 2px;
  margin-left: 2px;
  margin-right: 2px;
}
</style>

<template>
  <div class="main">
      <div class="searchBy flex">
          <p>全部结果 ></p>
          <div class="flex wrap">
              <div 
                v-for="(item,index) in searchBy"
                :key="index"
                v-show="item.list.length>0"
                class="searchItem all-center"
                :class="{active:item.isActive}">
                    <span class="cls">{{item.name}}:</span>
                    <span
                        v-for="(subItem,idx) in item.list"
                        :key="idx"
                        class="clsName">
                        {{subItem.text}}
                    </span>
                    <img 
                      src="/static/images/x.png"
                      @click="del(item)">
                </div>
          </div>
      </div>
      <div class="conditions">
          <div 
            v-for="(item,index) in conditions"
            :key="index"
            class="all-center item"
            :class="{multiChoose:item.multiChoose}">
                <div class="name">{{item.name}}:</div>
                <div class="fg ">
                    <div class="flex">
                       <div 
                        v-for="(subItem,idx) in item.list"
                        :key="idx"
                        class="subItem"
                        @click="singleCondition(item,subItem)">
                            <div>
                                <input 
                                type="checkbox" 
                                v-if="item.multiChoose" 
                                class="check" 
                                :value="subItem.text" 
                                v-model="selected"/>
                                <span>{{subItem.text}}</span>
                            </div>
                        </div>
                    </div>
                    <div class="flex btns" v-show="item.multiChoose">
                        <div class="sure" @click="handleSure(item)">确定</div>
                        <div class="cancel" @click="handleCancel(item)">取消</div>
                    </div>
                </div>
                
                <div class="multi" @click="showMulti(index)">+多选</div>
            </div>
      </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      searchBy: [
        {
          name: "品牌",
          list: []
        },
        {
          name: "大家说",
          list: []
        },
        {
          name: "硬盘容量",
          list: []
        },
        {
          name: "内存容量",
          list: []
        }
      ],
      conditions: [
        {
          name: "品牌",
          list: [
            {
              text: "thinkpad"
            },
            {
              text: "lenovo"
            },
            {
              text: "dell"
            }
          ],
          multiChoose: false
        },
        {
          name: "大家说",
          list: [
            {
              text: "开机速度"
            },
            {
              text: "性价比高"
            },
            {
              text: "散热很好"
            }
          ],
          multiChoose: false
        },
        {
          name: "硬盘容量",
          list: [
            {
              text: "1T"
            },
            {
              text: "128G+1T"
            },
            {
              text: "128G+500G"
            }
          ],
          multiChoose: false
        },
        {
          name: "内存容量",
          list: [
            {
              text: "2G"
            },
            {
              text: "4G"
            },
            {
              text: "8G"
            }
          ],
          multiChoose: false
        }
      ],
      selected: []
    };
  },
  methods: {
    singleCondition(item, subItem) {
      if (!item.multiChoose) {
        this.searchBy.map(val => {
          if (val.name === item.name) {
            val.list = [];
            val.list.push({ text: subItem.text });
          }
        });
      }
    },
    del(item) {
      item.list = [];
    },
    showMulti(idx) {
      this.selected = [];
      this.conditions.map((val, index) => {
        if (index === idx) {
          val.multiChoose = true;
        } else {
          val.multiChoose = false;
        }
      });
    },
    handleSure(item) {
      let selected = this.selected;
      this.searchBy.map(val => {
        if (val.name === item.name) {
          for (let i = 0; i < selected.length; i++) {
            val.list.push({ text: selected[i] });
          }
        }
      });
      item.multiChoose = false;
    },
    handleCancel(item) {
      item.multiChoose = false;
    }
  }
};
</script>

<style lang="scss" scoped>
.main {
  width: 80%;
  margin: 0 auto;
}
.searchBy {
  padding: 10px;
  padding-left: 0;
  .wrap{
      flex-wrap: wrap;
  }
  & > p {
      width:120px;
    padding: 5px;
    font-weight: bold;
  }
  .searchItem {
    padding: 3px 20px;
    padding-right: 5px;
    border: 1px solid #ddd;
    color: #e4393c;
    margin: 0 5px;
    background: #f3f3f3;
    font-size: 14px;
    cursor: pointer;
    img {
      max-height: 20px;
      margin-left: 10px;
    }
    &:hover {
      border: 1px solid #e4393c;
    }
    .clsName{
        margin: 0 5px;
    }
  }
}

.conditions {
  border-top: 1px solid #ddd;

  .item {
    border-bottom: 1px solid #ddd;
    height: 50px;
    .name {
      font-size: 16px;
      font-weight: bold;
      background: #f3f3f3;
      //   padding: 10px;
      width: 100px;
      height: 100%;
      line-height: 50px;
    }
    .subItem {
      color: lightblue;
      margin: 0 20px;
      cursor: pointer;
      .check {
        margin-right: 5px;
      }
    }

    .multi {
      padding: 3px 10px;
      border: 1px solid #ddd;
      cursor: pointer;
      letter-spacing: 3px;
      font-size: 14px;
      &:hover {
        color: #e4393c;
        border-color: #e4393c;
      }
    }
  }
  .multiChoose {
    height: 100px;
    .name {
      line-height: 100px;
    }
  }
}
.btns {
  padding: 10px;
  margin-bottom: -20px;
  div {
    padding: 5px 15px;
    background: #e4393c;
    color: #fff;
    margin: 0 15px;
    border-radius: 10px;
    font-size: 12px;
    cursor: pointer;
  }
  .sure {
    margin-left: 60%;
  }
  .cancel {
    background: #ddd;
    color: #fff;
  }
}
</style>



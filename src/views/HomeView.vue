<template>
  <div>
    <table border="1">
    <tr>
        <th>字段名称</th>
        <th>字段类型</th>
        <th>是否必填</th>
        <th>字段选项</th>
        <th>操作</th>
    </tr>
    <tr v-for="(item,index) in list" :key="index">
        <td>{{item.name}}</td>
        <td>{{item.required}}</td>
        <td>{{item.is}}</td>
        <td>
        <a-select v-model:value="value" style="width: 120px" v-if="item.required == '单选下拉'">
          <a-select-option  v-for="(items,indexs) in item.fieldoption" :key="indexs" :value="items.value">{{items.value}}</a-select-option>
        </a-select> 
        <div v-else>{{item.fieldoption}}</div>
        </td>
        <td><button @click="showModal(index)">编辑</button></td>
    </tr>
</table>

<a-modal v-model:visible="visible" width="1000px" :confirmLoading="false" :bodyStyle="{'display':'flex','flex-direction': 'column'}" title="编辑" >
       <a-form name="customized_form_controls" :model="aaa" :label-col="{ span: 8 }" :wrapper-col="{ span: 16 }" layout="vertical"  @finish="onFinish">

       <a-form-item 
      name="name"
       :wrapper-col="{ offset: 8, span: 8 }">
    名称： <a-input v-model:value="aaa.name"/>
    </a-form-item>

    <a-form-item
      name="required"
       :wrapper-col="{ offset: 8, span: 8 }">
    类型：  <a-select :value="aaa.required" @change="selectiveType">
              <a-select-option value="单行文本">单行文本</a-select-option>
              <a-select-option value="日期">日期</a-select-option>
              <a-select-option value="单选下拉">单选下拉</a-select-option>
            </a-select>
    </a-form-item>

    <a-form-item
      name="is"
        style="display: flex;" :wrapper-col="{span: 8, offset: 8}">
     <div style="display: flex;align-items: flex-start;">  
       <div>是否必填：</div> 
     <div>
       <a-radio-group v-model:value="aaa.is">
        <a-radio :style="radioStyle" :value="true">必须</a-radio>
        <a-radio :style="radioStyle" :value="false">非必须</a-radio>
      </a-radio-group>
     </div>
     </div>
    </a-form-item>

              <a-form-item
                name="fieldoption"
                v-if="aaa.required == '单行文本'" :wrapper-col="{ offset: 8, span: 8 }">
                      内容： <a-input v-model:value="aaa.fieldoption" placeholder="Basic usage" />
                    </a-form-item>

           <a-form-item v-else-if="aaa.required == '日期'" :wrapper-col="{ offset: 8, span: 8 }">
          <div style="display: flex;align-items: flex-start;">
             <div>日期: </div>
              <div>
                      <a-radio-group v-model:value="aaa.fieldoption">
                <a-radio :style="radioStyle" value="年-月">年-月</a-radio>
                <a-radio :style="radioStyle" value="年-月-日">年-月-日</a-radio>
                <a-radio :style="radioStyle" value="年-月-日-分-秒">年-月-日-分-秒</a-radio>
              </a-radio-group>
              </div>
          </div>
            </a-form-item>

            <a-form-item
                name="fieldoption"
                 v-else :wrapper-col="{ offset: 8, span: 8 }">
             下拉： <div v-for="(items,indexs) in aaa.fieldoption" :key="indexs" class="fieldoptionitem">
             {{items.value}}
             <div @click="del(indexs)">X</div>
               </div>
              <a-input v-model:value="text" placeholder="Basic usage" />  <a-button type="primary" @click="add(text)">添加</a-button>
            </a-form-item>

            <a-form-item
                name="fieldoption">
              <a-button type="primary" html-type="submit">Submit</a-button>
            </a-form-item>

        </a-form>
    </a-modal>

  </div>
</template>
<script lang="ts">
import { defineComponent, reactive, ref,toRefs } from 'vue';
export default defineComponent({
  setup() {
    let list = reactive([
        {
         name:'字段一',
         required:'单行文本',
         is:true,
         fieldoption:'诶嘿'
        },
        {
         name:'字段二',
         required:'日期',
         is:false,
         fieldoption:'年-月-日'
        },
        {
         name:'字段三',
         required:'单选下拉',
         is:false,
         fieldoption:[{
        value: 'jack',
        label: 'Jack (100)',
      },
      {
        value: 'lucy',
        label: 'Lucy (101)',
      },]
        }
    ])

let indextype = 0

     const visible = ref<boolean>(false);

    const datas = reactive<any>({aaa:{}})
    
    const text = ref<any>();

    const radioStyle = reactive({
      display: 'flex',
      height: '30px',
      lineHeight: '30px',
    });

   const selectiveType =(value:any, option:any)=>{
      console.log('e',value,option)
      if(value == '单选下拉'){
        datas.aaa.fieldoption =[{
        value: 'jack',
        label: 'Jack (100)',
      },
      {
        value: 'lucy',
        label: 'Lucy (101)',
      },]
      }
      datas.aaa.required = value
    }

     const showModal = (index:number) => {
      //  console.log('showModal',datas.aaa)
       datas.aaa = JSON.parse(JSON.stringify(list[index]))
       indextype = index
      //  console.log('showModal',datas.aaa)
      visible.value = true;
    };

     const formState = reactive({
      price: {
        number: 0,
        currency: 'rmb',
      },
    });
    const onFinish = (values: any) => {
      console.log('Received values from form: ', values,indextype);
      list[indextype] = values
      visible.value = false;
    };
    const checkPrice = (_: any, value: { number: number }) => {
      if (value.number > 0) {
        return Promise.resolve();
      }
      return Promise.reject(new Error('Price must be greater than zero!'));
    };
    const del =(index:number)=>{
      console.log(index,datas.aaa.fieldoption);
      datas.aaa.fieldoption.splice(index,1)
    }
    const add =(e:string)=>{
      console.log(e);
      datas.aaa.fieldoption.push({'value':e,'label':e+'(100)'})
    }
    return {
     list,
     visible,
     radioStyle,
    ...toRefs(datas),
     formState,
     text,
     indextype,
     value: ref({ value: 'lucy' }),
     selectiveType,
     showModal,
      onFinish,
     checkPrice,
     del,
     add
    };
  },
});
</script>
<style scoped>
.fieldoptionitem{
  padding: 20px;
  background-color: #f2f3f5;
  display: inline-block;
  margin: 10px;;
  position: relative;
}
.fieldoptionitem div{
  position: absolute;
  top: 0;
  right: 0;
}
</style>

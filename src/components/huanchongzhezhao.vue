<style scoped>
    #add{
        width: 100%;
        height: 100%;
        position: fixed;
        z-index: 3;
        background-color: #0d0d0d;
        color: #ffffff;
        display: flex;
        justify-content: center;
        align-items: center;
    }
    .progress{
        width: 100px;
        height: 2px;
        /* color: #ffffff; */
        border: 1px solid #ffffff;
    }
</style>

<script lang="ts">
import { defineComponent,ref,onMounted } from 'vue'
export default {
  props: {
      isShow:{
        type: Boolean,
        default: false,
      }
  },
  setup(props) {
    // setup() 接收 props 作为第一个参数
    console.log(props.isShow)
    let isShows = ref(props.isShow)
    return{
        isShows
    }  
    onMounted(()=>{

    })
  },
  mounted() {

  },
  methods: {
  },
}
</script>

<template>
  <div id="add" v-if="isShow">
      <div class="flex flex-j-content-ctr flex-a-ies-ctr flex-dcn-cln">
          <div>Loading···</div>
          <div class="progress"></div>
      </div>
  </div>
</template>



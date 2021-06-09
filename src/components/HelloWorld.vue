<template>
  <div class="message-Container">
    <div class="massage-Wrapper">
      <div class="massage-Part">
        <TextEditor
          ref="text-editor"
          v-model="message"
          :users="userdata"
          :placeholder="message"
          class="massage comment-editor h-ease-15 h-10"
          @mentions="getForwardList"
        >
          <div class="massageBtn" contenteditable="false">
            <button>Buraya Tıkla</button>
          </div>
        </TextEditor>
      </div>
    </div>
  </div>
</template>

<script>
import TextEditor from "./TextEditor.vue";
import userdata from "./userdata";

export default {
  data(){
    return{
      userdata:[],
    }
  },
  components: {
    TextEditor,
  },
  created(){
    this.userdata=userdata;
  }
};
</script>

<style>
.message-Container {
  width: 100%;
  height: 100%;
  text-align: -webkit-center;
}
.massage-Wrapper {
  justify-content: center;
  align-items: center;
  display: flex;
  width: 24rem;
  /* text-align: center; */
  height: 24rem;
  background: cadetblue;
}
.massage {
  border: 1px solid wheat;
  background: white;
  width: 20rem;
  height: 17rem;
}
.editor-input{
  width: 100%;
  height: 100%;
  text-align: start;
  padding:2%
}
</style>

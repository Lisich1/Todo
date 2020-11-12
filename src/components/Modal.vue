<template>
    <div  id="modal1" class="modal container" v-if="modalInfo.isVisible">
        <div class="modal-content">
            <a>{{modalInfo.contentText}}</a>
        </div>
        <div  class="modal-footer container" >
          <div class="col s12">
            <div class="col s8"></div>
            <div class="col s2">
              <button  id="delete"  class="delete btn waves-effect waves-light purple lighten-2 " v-if="modalInfo.isChoice"  v-on:click="cancel">{{modalInfo.cancelText}}</button>
            </div>
            <div class="col s2">
              <button class="modal-close btn waves-effect waves-light purple lighten-2" v-on:click="submit">{{modalInfo.submitText}}</button>
            </div>
          </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "Modal",
    props: {
        modalInfo: {
            name: String,
            isChoice: Boolean,
            submitText: String,
            cancelText: String,
            contentText: String,
            submitFunc: Function,
            isVisible: Boolean
        }
    },
    methods: {
        cancel  (){
            this.modalInfo.isVisible = false;
        },
        submit  (){
            this.modalInfo.isVisible = false;
            this.modalInfo.submitFunc();
        }
    }
}
</script>

<style>

.modal {
  display: block;
  top: 41%;
  width: 50%;
  z-index: 100;
}
.modal-footer {
  display: block;
}
</style>

<template>
  <div
    class="noteBox"
    draggable="true"
    @dragstart="drag_start"
    @dragend="drop"
    :style="{ position: noteObj.postion, left: noteObj.left, top: noteObj.top }"
  >

    <b-form-textarea
      :value="noteObj.note"
      :name="noteObj.id"
      @change="onChangeVal"
      class="textBox"
      :style="{ background : noteObj.color }"
      id="textarea"
      placeholder="Enter Note Text Here"
      rows="3"
      max-rows="6"
    >
    </b-form-textarea>
  </div>
</template>

<script>
export default {
  name: "AddNote",
  props: {
    noteObj: Object,
  },

  methods: {
    onChangeVal: function (sNewVal) {

      this.noteObj.note = sNewVal;
      this.$emit("updateNote", this.noteObj );
    },

    drag_start: function (event) {
      var style = window.getComputedStyle(event.target, null);
      this.startX = parseInt(style.getPropertyValue("left"), 10) - event.clientX;
      this.startY = parseInt(style.getPropertyValue("top"), 10) - event.clientY ;
    },

    drag_over: function (event) {
      event.preventDefault();
      return false;
    },

    drop: function (event) {
      this.noteObj.left = event.clientX + parseInt(this.startX, 10) + "px";
      this.noteObj.top = event.clientY + parseInt(this.startY, 10) + "px";
      console.log(this.noteObj.left + "," + this.noteObj.top);
      this.$emit("updateNote", this.noteObj );
     
      event.preventDefault();
      return false;
    },
  },
  
};
</script>

<style>
.noteBox {
  width: 25%;
  height: 50%;
  border: 0.1rem solid black !important;
}
.textBox
{
  height: 100% !important;
}
</style>
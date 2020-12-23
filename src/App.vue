<template>
  <div class="paddCss">
    <b-button variant="outline-primary" @click="onAddNoteReq">
      Add Note
    </b-button>
    <br />

    <AddNote
      v-for="x in notes"
      :noteObj="x"
      @updateNote="updateNotefn"
      :key="x.id"
    ></AddNote>
  </div>
</template>

<script>
import AddNote from "./components/AddNote";

export default {
  name: "App",
  data() {
    return {
      notes: [{ note: "Note 1", id: "0", postion: "fixed", left: "500px", top: "200px",
      color : "#DFECFE" }],
    };
  },
  components: {
    AddNote,
  },
  methods: {
    onAddNoteReq: function () {
      let len = Number(this.notes.length);
      len = ++len;
      let newStr = "Note " + len;
      var leftX = String(parseInt(Math.random()*100) * 2).concat("px");
      var leftY = String(parseInt(Math.random()*100)* 5).concat("px");
      console.log(leftX+","+leftY);
      let sColor = this.getRandomColor();
      this.notes.push({ id: String(len), note: newStr, postion: "fixed", left: leftX, top: leftY, color: sColor });
      localStorage.setItem("notes", JSON.stringify(this.notes));
    },

     getRandomColor: function() {
                var letters = 'BCDEF'.split('');
                var color = '#';
                for (var i = 0; i < 6; i++ ) {
                    color += letters[Math.floor(Math.random() * letters.length)];
                }
                return color;
            },

    updateNotefn: function (Obj) {
    
      var sIndex = "";
      for (var i = 0; i < this.notes.length; i++) {
        if (this.notes[i].id === Obj.id) {
          sIndex = i;
          break;
        }
      }
      if (sIndex !== "") {
        // this.notes[index].note = newValue;
        this.notes = this.notes.map((item, index) => {
          if (index !== sIndex) {
           
            return item;
          }          
          return {
            ...item,
            ...Obj,
          };
        });
        localStorage.setItem("notes", JSON.stringify(this.notes));
      }
    },

  },
  beforeMount() {
    console.log("Before Mount Call");
    if (JSON.parse(localStorage.getItem("notes")) !== null)
      this.notes = JSON.parse(localStorage.getItem("notes"));
    console.log(JSON.parse(localStorage.getItem("notes")));
  },
  updated() {
    console.log("updated Call");
    console.log(this.notes);
  },
  computed: {},
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.paddCss
{
  padding : 1rem;
}
</style>

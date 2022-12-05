<template>
  <div :class="isReduceClass()">
    <div class="firstLine line">
      <div id="firstLineLeft"><strong>Nouveau Message</strong></div>
      <div id="firstLineEmpty"></div>
      <div id="firstLineRight">
        <span class="material-icons" v-on:click="reduce"> remove </span>
        <span class="material-icons" v-if="isNotReduced"> fullscreen </span>
        <span class="material-icons" v-if="isNotReduced" v-on:click="close">
          close
        </span>
      </div>
    </div>
    <div v-if="isNotReduced" class="line">
      <input type="text" v-model="author" placeholder="Destinataire" />
    </div>
    <div v-if="isNotReduced" class="line">
      <input type="text" v-model="object" placeholder="Objet" />
    </div>
    <div>
      <textarea v-if="isNotReduced" v-model="content"></textarea>
    </div>
    <div v-if="isNotReduced" class="lastLine">
      <button class="sendButton" v-on:click="send">
        Envoyer&nbsp;&nbsp;&nbsp;
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: "EmailWrite",
  props: {
    author: String,
    object: String,
    content: String,
  },
  data() {
    return {
      cont: "",
      isNotReduced: true,
    };
  },
  methods: {
    close: function () {
      this.$emit("close-mail");
    },
    reduce: function () {
      this.isNotReduced = !this.isNotReduced;
      console.log("switch reduce ");
      console.log(this.isReduceClass());
    },
    send: function () {
      if(this.author.length == 0 ) return alert("destinataire vide");
      if(this.object.length == 0 ) return alert("objet du mail vide");
      if(this.content.length == 0 ) return alert("mail vide ...");

      let msgFinal =
        "author:" + this.aut + " obj:" + this.obj + " content:" + this.con;
      console.log("send message");
      console.log(msgFinal);
      let data = [this.aut, this.obj, this.con];
      this.$emit("send-mail", data);

    },
    isReduceClass: function () {
      if (this.isNotReduced) return "writeMain";
      else return "reduceMain";
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.material-icons:hover {
  cursor: pointer;
}
.writeMain {
  position: absolute;
  width: 500px;
  height: 500px;
  margin: 50px auto auto 0px;
  right: 5px;
  bottom: 0px;
  text-align: center;
  border-style: solid;
  border-color: rgb(191, 191, 191);
  background-color: rgb(255, 255, 255);
  border-radius: 4px;
}

.reduceMain {
  position: absolute;
  width: 300px;
  height: 30px;
  margin: 50px auto auto 0px;
  right: 5px;
  bottom: 0px;
  text-align: center;
  border-style: solid;
  border-color: rgb(191, 191, 191);
  background-color: rgb(255, 255, 255);
  border-radius: 4px;
}

.firstLine div {
  float: left;
  width: 100%;
}

.writeMain line {
  position: relative;
  padding-left: 1em;
  text-align: left;
  height: 30px;
}

.writeMain textarea {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  width: 100%;
  height: 350px;
  outline: none;
  resize: none;
  overflow: auto;
}

.writeMain input {
  border-top: none;
  border-left: none;
  border-right: none;
  border-color: rgb(231, 231, 231);
  width: 90%;
  padding: 10px;
  padding-bottom: 3px;
}

.firstLine {
  background-color: rgb(239, 215, 224);
  text-align: left;
}

.firstLine:after {
  content: "";
  display: table;
  clear: both;
}

#firstLineLeft {
  float: left;
  text-align: left;
  width: 50%;
}

#firstLineEmpty {
  text-align: right;
  width: 28%;
  border-color: rgba(92, 56, 90, 0);
  border-style: solid;
}

#firstLineRight {
  float: left;
  text-align: right;
  width: 20%;
}

.lastLine {
  height: 30px;
  text-align: left;
  vertical-align: top;
}

.sendButton {
  margin-top: 2px;
  margin-left: 10px;
  padding-right: 100px;
  margin-bottom: 15px;
  text-transform: capitalize;
  color: gray;
  padding: 15px;
  border-radius: 30px;
  background-color: rgb(236, 202, 202);
  display: flex;
  align-items: center;
  cursor: pointer;
  outline: none;
  border: none;
  box-shadow: 0px 2px 5px -2px rgba(0, 0, 0, 0.75);
}

.sendButton:hover {
  box-shadow: 0px 2px 5px 2px #969696;
  font-weight: bolder;
}
</style>

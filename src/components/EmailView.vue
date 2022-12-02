<template>
  <div class="emailMain">
    <div class="emailHeader">
      <span class="material-icons" v-on:click="leaveView">arrow_back </span>
      <span class="material-icons"> move_to_inbox</span>
      <span class="material-icons"> error_out</span>
      <span class="material-icons"> delete </span>
      <span class="material-icons"> mail </span>
      <span class="material-icons"> access_time</span>
      <span class="material-icons"> playlist_add_check</span>
      <span class="material-icons"> create_new_folder</span>
      <span class="material-icons"> label_outline </span>
      <span class="material-icons"> more_vert </span>
    </div>
    <div class="emailObject">
      {{ object }}
    </div>
    <div class="emailAuthor">
      <div class="circlePeople">&nbsp;</div>
      <div class="emailAuthor_title">
        <strong>' {{ author }}' </strong> nov. 2022 22:54 ( il y a 5 jours )
        <p>A moi</p>
      </div>
      <div class="emailAuthor_settings">
        <span class="material-icons"> star</span>
        <span class="material-icons"> arrow_back</span>
        <span class="material-icons"> more_vert </span>
      </div>
    </div>
    <div class="emailView" v-html="content">
      
    </div>
  </div>
</template>

<script>
export default {
  name: "EmailView",
  props: {
    author: String,
    object: String,
    content: String,
  },
  data() {
    return {
      selectAllCheckbox: false,
    };
  },
  methods: {
    leaveView: function () {
      console.log("leave the view");
      this.$emit("leave-view");
    },
    /*
    TODO : écrire un emit mailAction avec toutes les infos qu'on a besoin, "quelle action", "quel parametre". Comme ça, un unique emit à remonter juq'au
    main, pour toutes actions possibles sur les mails.
    */
  },
  watch: {},
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.emailMain {
  flex: 1;
  margin-top: 10px;
  padding: 20px;
}

.emailHeader {
  text-align: left;
}

.emailHeader .material-icons {
  cursor: pointer;
  width: 23px;
  margin-right: 20px;
}
.emailObject {
  flex: 1;
  padding: 10px;
  text-align: left;
  margin-top: 20px;
  margin-bottom: 20px;
  font-size: 2em;
}

.emailAuthor {
  text-align: left;
  vertical-align: middle;
  display: table;
  width: 100%;
}

.emailAuthor_title {
  width: 50%;
  margin: 0px;
  float: left;
}

.emailAuthor_settings {
  text-align: right;
  width: 20%;
  float: right;
  margin-right: 0px;
}

.emailAuthor_settings .material-icons {
  margin-right: 10px;
}

.emailAuthor:after {
  content: "";
  display: table;
  clear: both;
}

.circlePeople {
  border-radius: 25px;
  width: 50px;
  height: 50px;
  background-color: red;
  float: left;
  text-align: left;
}
.emailView {
  flex: 1;
  overflow: hidden;
  padding: 10px;
}
</style>

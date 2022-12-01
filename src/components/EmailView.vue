<template>
  <div class="emailMain">
    <div class="emailHeader">
      <span class="material-icons icon-click" v-on:click="leaveView">
        arrow_back
      </span>
      Header
    </div>
    <div class="emailObject">
      <h3>{{ object }}</h3>
    </div>
    <div class="emailAuthor">
      <div class="circlePeople">&nbsp;</div>
      <div>
        <div>
          <strong>' {{ author }}' </strong> nov. 2022 22:54 ( il y a 5 jours )
        </div>
        <div>A moi</div>
      </div>
    </div>
    <div class="emailView">
      <p>content = {{ content }}</p>
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
  },
  watch: {},
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.iconClick {
  cursor: pointer;
}

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
}
.emailObject {
  flex: 1;
  padding: 10px;
}
.emailAuthor {
  text-align: left;
  table-layout: fixed;
  vertical-align: middle;
  flex: 1;
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

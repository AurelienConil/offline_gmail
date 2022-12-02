<template>
  <div class="header">
    <div class="header__left">
      <span class="material-icons"> menu </span>
      <img
        src="~@/assets/gmail.png"
        alt=""
      />
    </div>

    <div class="header__middle">
      <span class="material-icons"> search </span>
      <input type="text" v-model="searchMsg" placeholder="Search mail" />
      <span class="material-icons" v-if="searchMsg.length == 0">
        arrow_drop_down
      </span>
      <span class="material-icons" v-else  v-on:click="deleteSearch">
        close
      </span>
    </div>

    <div class="header__right">
      <span class="material-icons"> help_outline </span>
      <span class="material-icons"> settings </span>
      <span class="material-icons"> apps </span>
      <span class="material-icons"> account_circle </span>
    </div>
  </div>
</template>

<script>
export default {
  name: "Header",
  props: {
    msg: String,
  },
  data() {
    return {
      searchMsg: "",
    };
  },
  methods:{
    deleteSearch: function () {
      this.searchMsg = "";
    },
  },
  watch: {
    searchMsg: function () {
      this.$emit("search", this.searchMsg);
    },
    msg: function () {
      //this.searchMsg=this.msg;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<!-- @import "https://fonts.googleapis.com/icon?family=Material+Icons";-->
<!-- @import '~material-design-icons/iconfont/material-icons.css';-->
<style scoped>

@import '~material-design-icons/iconfont/material-icons.css';

.header__left img {
  object-fit: contain;
  height: 80px;
  margin-left: 5px;
}

.header__right .material-icons{
  margin:10px
}

.header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  border-bottom: 5px solid rgb(107, 55, 55);
  border: 0px;
}

.header__left {
  display: flex;
  align-items: center;
}

.header__left span {
  padding: 10px;
  cursor: pointer;
}

.header__middle {
  display: flex;
  flex: 0.7;
  align-items: center;
  background-color: whitesmoke;
  padding: 10px;
  border-radius: 5px;
}

.header__middle input {
  border: none;
  width: 100%;
  padding: 10px;
  outline: none;
  font-size: medium;
  background-color: transparent;
}

.material-icons {
  color: gray;
}
</style>

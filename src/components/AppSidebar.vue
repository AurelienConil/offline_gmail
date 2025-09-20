<template>
  <div class="sidebar">
    <button class="sidebar__compose" v-on:click="newMail">
      <span class="material-icons"> border_color </span> Nouveau message
    </button>

    <div class="sidebarOption " :class="{sidebarOption__active: currentMenu(0)}" v-on:click="inbox">
      <span class="material-icons"> inbox </span>
      <h3>Boîte de réception</h3>
    </div>
    <div class="sidebarOption"  :class="{sidebarOption__active: currentMenu(1)}" v-on:click="starred">
      <span class="material-icons"> star </span>
      <h3>Favoris</h3>
    </div>
    <div class="sidebarOption" :class="{sidebarOption__active: currentMenu(2)}" v-on:click="snoozed" >
      <span class="material-icons"> access_time </span>
      <h3>En attente</h3>
    </div>
    <div class="sidebarOption" :class="{sidebarOption__active: currentMenu(3)}" v-on:click="important">
      <span class="material-icons" style="width:20px"> label_important </span>
      <h3>Important</h3>
    </div>

    <div class="sidebarOption" :class="{sidebarOption__active: currentMenu(4)}" v-on:click="sent">
      <span class="material-icons"> near_me </span>
      <h3>Messages envoyés</h3>
    </div>

    <div class="sidebarOption" :class="{sidebarOption__active: currentMenu(5)}" v-on:click="draft">
      <span class="material-icons"> note </span>
      <h3>Brouillons</h3>
    </div>

    <div class="sidebarOption">
      <span class="material-icons"> expand_more </span>
      <h3>Plus</h3>
    </div>

    <div class="sidebar__footer">
      <div class="sidebar__footerIcons">
        <span class="material-icons"> person </span>
        <span class="material-icons"> perm_camera_mic </span>
        <span class="material-icons"> phone </span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "AppSidebar",
  props: {
    msg: String,
  },
  methods:{
    newMail : function(){
      this.$emit("new-mail");
    },
    inbox : function(){
      this.menu = 0;
      this.$emit("sidebar-inbox");
    },
    starred : function(){
      console.log("click on star")
      this.$emit("sidebar-starred");
      this.menu = 1;
    },
    snoozed : function(){
      this.menu = 2;
      this.$emit("sidebar-snoozed");
    },
    important : function(){
      this.menu = 3;
      this.$emit("sidebar-important");
    },
    sent : function(){
      this.menu = 4;
      this.$emit("sidebar-sent");
    },    
    draft : function(){
      this.menu = 5;
      this.$emit("sidebar-draft");
    },
    currentMenu : function(index){
        return index==this.menu;
    }

  },
  data(){
    return {
      menu : 0
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.sidebar {
  flex: 0.3;
  max-width: 300px;
  padding-right: 20px;
}

.sidebar__compose {
  margin-top: 15px;
  margin-left: 10px;
  margin-bottom: 15px;
  text-transform: capitalize;
  color: gray;
  padding: 15px;
  border-radius: 30px;
  background-color: white;
  display: flex;
  align-items: center;
  cursor: pointer;
  outline: none;
  border: none;
  box-shadow: 0px 2px 5px -2px rgba(0, 0, 0, 0.75);
}

.sidebar__compose:hover{
  box-shadow: 0px 2px 5px 0px rgba(0, 0, 0, 0.75);
}

.sidebar__compose span {
  margin-right: 5px;
}

.sidebarOption {
  display: flex;
  align-items: center;
  height: 40px;
  padding: 0 10px;
  border-top-right-radius: 20px;
  border-bottom-right-radius: 20px;
  cursor: pointer;
  color: #818181;
}

.sidebarOption .material-icons {
  padding: 5px;
}

.sidebarOption h3 {
  flex: 1;
  margin-left: 10px;
  font-size: 14px;
  font-weight: 400;
}

.sidebarOption:hover,
.sidebarOption:hover h3,
.sidebarOption:hover .material-icons,
.sidebarOption__active,
.sidebarOption__active h3,
.sidebarOption__active .material-icons {
  background-color: #fcecec;
  color: #c04b37;
  font-weight: 800;
}

.sidebar__footer {
  display: flex;
  justify-content: center;
}

.sidebar__footerIcons .material-icons {
  margin: 7px;
  cursor: pointer;
}
</style>

<template>
  <div class="emailList">
    <!-- Settings Starts -->
    <div class="emailList__settings">
      <div class="emailList__settingsLeft" v-if="isSelection">
        <input type="checkbox" v-model="selectAllCheckbox" />
        <span class="material-icons"> arrow_drop_down </span>
        <span class="material-icons" title="déplacer dans boite de reception" > move_to_inbox</span>
        <span class="material-icons" title="marquer comme spam"> error_out</span>
        <span class="material-icons" title="supprimer" v-on:click="deleteSelected"> delete </span>
        <span class="material-icons" title="marquer comme non lu" > mail </span>
        <span class="material-icons"> access_time</span>
        <span class="material-icons"> playlist_add_check</span>
        <span class="material-icons"> create_new_folder</span>
        <span class="material-icons"> label_outline </span>
        <span class="material-icons"> more_vert </span>
      </div>
      <div class="emailList__settingsLeft" v-else>
        <input type="checkbox" v-model="selectAllCheckbox" />
        <span class="material-icons"> arrow_drop_down </span>
        <span class="material-icons"> refresh </span>
        <span class="material-icons"> more_vert </span>
      </div>
      <div class="emailList__settingsRight">
        <span class="material-icons"> chevron_left </span>
        <span class="material-icons"> chevron_right </span>
      </div>
    </div>
    <!-- Settings Ends -->

    <!-- Section Starts -->
    <div class="emailList__sections">
      <div class="section section__selected">
        <span class="material-icons"> inbox </span>
        <h4>Primary</h4>
      </div>

      <div class="section">
        <span class="material-icons"> people </span>
        <h4>Social</h4>
      </div>

      <div class="section">
        <span class="material-icons"> local_offer </span>
        <h4>Promotions</h4>
      </div>
    </div>
    <!-- Section Ends -->

    <!-- Email List rows starts -->
    <div class="emailList__list">
      <!-- Email Row Starts -->
      <Email
        v-for="(item, i) in listofmail"
        :author="item.author"
        :key="i"
        :index="i"
        :time="item.time"
        :preview="item.preview"
        :read="item.read"
        :selected="item.selected"
        :starred="item.starred"
        :important="item.important"
        v-on:element-selection="elementSelected"
        v-on:element-starred="elementStarred"
        v-on:element-important="elementImportant"
        v-on:element-open="elementOpen"
      >
      </Email>
    </div>
  </div>
</template>

<script>
import Email from "./Email.vue";
export default {
  name: "Sidebar",
  components: {
    Email,
  },
  props: {
    listofmail: Array,
  },
  data() {
    return {
      selectAllCheckbox: false,
      isSelection: true,
    };
  },
  methods: {
    elementSelected: function (data) {
      this.isSelection = data[1];
      this.$emit("list-selection", data);
    },
    elementStarred: function (data) {
      this.$emit("element-starred", data);
    },
    elementImportant: function (data) {
      this.$emit("element-important", data);
    },
    deleteSelected: function () {
      this.$emit("delete-selected");
    },
    elementOpen: function (arg) {
      this.$emit("element-open", arg);
    },
  },
  watch: {
    selectAllCheckbox(val) {
      this.$emit("select-all", val);
      this.isSelection = val;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.emailList {
  flex: 1;
}

.emailList__settings {
  position: sticky;
  top: 0;
  display: flex;
  justify-content: space-between;
  border-bottom: 1px solid whitesmoke;
  background-color: white;
  padding: 8px;
}

.emailList__settingsLeft {
  display: flex;
  align-items: center;
}

.emailList__settings .material-icons {
  margin-left: 10px;
  margin-right: 10px;
  width: 23px;
  cursor: pointer;
}

.section {
  display: flex;
  align-items: center;
  border-bottom-width: 2px;
  padding: 15px;
  min-width: 200px;
  cursor: pointer;
  color: grey;
  border-width: 0;
}

.emailList__sections {
  position: sticky;
  top: 0;
  display: flex;
  background-color: white;
  border-bottom: 1px solid whitesmoke;
}

.emailList__list {
  overflow: scroll;
  overflow-x: auto;
  scroll-padding: 0;
  height: 75vh;
}

.section__selected {
  background-color: whitesmoke;
  border-width: 3px;
  color: red;
  border-bottom: 3px solid red;
}

.section__selected .material-icons {
  color: red;
}

.section:hover {
  background-color: whitesmoke;
  border-width: 3px;
}

.section h4 {
  font-size: 14px;
  margin-left: 15px;
}
</style>

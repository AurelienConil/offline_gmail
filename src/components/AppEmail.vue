<template>
  <div class="emailRow" :class="backgroundLine()">
    <div class="emailRow__options">
      <input
        type="checkbox"
        name=""
        id=""
        v-model="isSelected"
        @change="selectedMail"
      />
      <span class="material-icons" v-if="starred" v-on:click="starredMail">
        star</span
      >
      <span class="material-icons" v-else v-on:click="starredMail">
        star_border
      </span>
      <span class="material-icons" v-if="important" v-on:click="importantMail">
        label
      </span>
      <span class="material-icons" v-else v-on:click="importantMail">
        label_outline
      </span>
    </div>
    <p class="emailRow__title" :class="{ unread: read }" v-on:click="clickMail">
      {{ getNameAuthor() }}
    </p>

    <div class="emailRow__message" v-on:click="clickMail">
      <p :class="{ unread: read }">
        {{ preview }}
      </p>

      <p></p>
    </div>

    <p class="emailRow__time">{{ time }}</p>
  </div>
</template>

<script>
export default {
  name: "AppEmail",
  props: {
    index: Number,
    author: String,
    time: String,
    preview: String,
    read: Boolean,
    selected: Boolean,
    starred: Boolean,
    important: Boolean,
  },
  data() {
    return {
      isSelected: false,
    };
  },
  methods: {
    selectedMail: function () {
      this.$emit("element-selection", [this.index, this.isSelected]);
    },
    starredMail: function () {
      this.$emit("element-starred", [this.index, this.starred]);
    },
    importantMail: function () {
      this.$emit("element-important", [this.index, this.important]);
    },
    clickMail: function () {
      this.$emit("element-open", this.index);
    },
    backgroundLine: function () {
      if (!this.read) return "readLine";
      else return "unreadLine";
    },
    getNameAuthor: function () {
      let split = this.author.split("@");
      let name = split[0];
      return name;
    },
  },
  watch: {
    selected(val, oldVal) {
      if (val !== oldVal) this.isSelected = val;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.emailRow__options {
  display: flex;
  align-items: center;
}

.unread {
  font-weight: bold;
}
.emailRow__options .material-icons,
input {
  margin: 2px;
}

.emailRow {
  display: flex;
  align-items: center;
  height: 50px;
  border-bottom: 1px solid whitesmoke;
  cursor: pointer;
  z-index: 999;
}

.emailRow:hover {
  border-top: 1px solid whitesmoke;
  box-shadow: 0 4px 4px -2px rgba(0, 0, 0, 0.24);
}

.emailRow__message {
  display: flex;
  flex: 0.8;
  align-items: center;
  font-size: 13px;
}

.emailRow__message h4 {
  width: 400px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  padding-left: 5px;
  padding-right: 5px;
  text-align: right;
}

.emailRow__title {
  font-size: 13px;
  flex: 0.3;
  text-align: left;
  margin: 10px;
}

.emailRow__time {
  padding-right: 15px;
  font-size: 10px;
  font-weight: bold;
}

.emailRow__description {
  font-weight: 400;
  color: gray;
}

.readLine {
  background-color: rgb(247, 247, 247);
}
.unreadLine {
  background-color: rgb(255, 255, 255);
}
</style>

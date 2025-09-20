<template>
  <div id="app">
    <EmailWrite
      v-if="writingEmail"
      :author="writeMail.author"
      :object="writeMail.object"
      :content="writeMail.content"
      v-on:close-mail="closeMail"
      v-on:reduce-mail="reduceMail"
      v-on:send-mail="sendMail"
    />
  <AppHeader :msg="currentSearch" v-on:search="search" />
    <div class="main__body">
  <AppSidebar
        v-on:new-mail="newMail"
        v-on:sidebar-starred="sidebarStarred"
        v-on:sidebar-sent="sidebarSent"
        v-on:sidebar-important="sidebarImportant"
        v-on:sidebar-inbox="sidebarInbox"
        v-on:sidebar-snoozed="sidebarSnoozed"
        v-on:sidebar-draft="sidebarDraft"
      />
      <EmailView
        v-if="readingEmail"
        :author="viewMail.author"
        :object="viewMail.object"
        :content="viewMail.content"
        :index="viewMail.index"
        :time="viewMail.time"
        v-on:leave-view="leaveView"
        v-on:action="actionOnOne"
        v-on:answer="answerMail"
      />
      <EmailList
        v-else
        :listofmail="currentlistofemail"
        v-on:list-selection="mailSelected"
        v-on:element-starred="mailStarred"
        v-on:element-important="mailImportant"
        v-on:select-all="selectAll"
        v-on:action="action"
        v-on:element-open="mailOpen"
      />
    </div>
  </div>
</template>

<script>
import AppHeader from "./components/AppHeader.vue";
import AppSidebar from "./components/AppSidebar.vue";
import EmailList from "./components/EmailList.vue";
import EmailView from "./components/EmailView.vue";
import EmailWrite from "./components/EmailWrite.vue";
import dataEmailInbox from "./emailDataInbox.json";
import dataEmailSent from "./emailDataSent.json";

export default {
  name: "App",
  components: {
    AppHeader,
    AppSidebar,
    EmailList,
    EmailView,
    EmailWrite,
  },
  data: () => ({
    readingEmail: false,
    writingEmail: false,
    writeMail: {
      author: "",
      object: "",
      content:""
    },
    viewMail: {
      author: "bab",
      object: "bibi",
      content: "Je cherche à me ",
      index: -1,
      time : "",
    },
    currentlistofemail: [],
    currentcategoryemail: "inbox",
    currentSearch: "",
    listofemailinbox: dataEmailInbox,
    listofemailsent: dataEmailSent,
  }),
  methods: {
    mailSelected: function (newSelected) {
      this.currentlistofemail[newSelected[0]].selected = newSelected[1];
    },
    mailImportant: function (select) {
      this.currentlistofemail[select[0]].important = !select[1]; // important if it is not important
    },
    mailStarred: function (star) {
      this.currentlistofemail[star[0]].starred = !star[1]; // star if it is not starred
    },
    mailOpen: function (index) {
      this.viewMail.author = this.currentlistofemail[index].author;
      this.viewMail.object = this.currentlistofemail[index].preview;
      this.viewMail.content = this.currentlistofemail[index].msg;
      this.viewMail.index = index;
      this.viewMail.time = this.currentlistofemail[index].time;
      this.currentlistofemail[index].read = false; // read means unread
      this.readingEmail = true;
    },
    sendMail: function (data) {
      let m = {};
      m.author = data[0];
      m.msg = data[2];
      m.time = "10pm";
      m.preview = data[1];
      m.read = true;
      m.starred = false;
      m.important = false;
      m.spam = false;
      this.listofemailsent.push(m);
      this.writeMail.author = "";
      this.writeMail.object = "";
      this.writeMail.content = "";
      this.writingEmail= false;
      console.log(data);
      alert("mail envoyé");

    },
    answerMail: function (arg) {
      let aut = arg[0];
      let obj = arg[1];
      this.writeMail.author = aut;
      this.writeMail.object = "RE : "+obj;
      this.readingEmail = false;
      this.writingEmail = true;
    },
    search: function (s) {
      this.currentSearch = s;
      this.resetCurrentList();
    },
    selectAll: function (isSelected) {
      this.currentlistofemail.forEach((el) => {
        el.selected = isSelected;
      });
    },
    leaveView: function () {
      this.readingEmail = false;
    },
    actionOnOne: function (arg) {
      this.selectAll(false);
      let index = arg[1];
      let act = arg[0];
      this.currentlistofemail[index].selected = true;
      this.action(act);
      this.selectAll(false);
      this.readingEmail = false;
    },
    action: function (actionName) {
      switch (actionName) {
        case "delete":
          this.currentlistofemail = this.currentlistofemail.filter(
            (item) => !item.selected
          );
          break;
        case "inbox":
          break;
        case "spam":
          this.currentlistofemail.forEach((element) => {
            if (element.selected) {
              element.spam = true;
            }
          });
          break;
        case "unread":
          this.currentlistofemail.forEach((element) => {
            if (element.selected) {
              element.read = true;
            }
          });
          break;
        case "important":
          this.currentlistofemail.forEach((element) => {
            if (element.selected) {
              element.important = true;
            }
          });
          break;
      }
    },
    newMail: function () {
      this.writingEmail = true;
    },
    closeMail: function () {
      this.writingEmail = false;
    },
    reduceMail: function () {
      console.log("to do");
    },
    sidebarInbox: function () {
      this.currentcategoryemail = "inbox";
      this.resetCurrentList();
    },
    sidebarStarred: function () {
      this.currentcategoryemail = "starred";
      this.resetCurrentList();
    },
    sidebarImportant: function () {
      this.currentcategoryemail = "important";
      this.resetCurrentList();
    },
    sidebarSent: function () {
      this.currentcategoryemail = "sent";
      this.resetCurrentList();
      console.log("sent mode activated");
    },

    sidebarSnoozed: function () {},
    sidebarDraft: function () {},
    resetCurrentList: function () {
      switch (this.currentcategoryemail) {
        case "inbox":
          this.currentlistofemail = this.listofemailinbox;
          this.readingEmail = false;
          break;
        case "starred":
          this.currentlistofemail = this.listofemailinbox.filter(
            (item) => item.starred
          );
          break;
        case "important":
          this.currentlistofemail = this.listofemailinbox.filter(
            (item) => item.important
          );
          break;
        case "sent":
          this.currentlistofemail = this.listofemailsent;
          break;
      }
      this.selectAll(false); // Unselect ALL after change of category
      if (this.currentSearch.length > 0) {
        this.currentlistofemail = this.currentlistofemail.filter(
          (item) =>
            item.author.includes(this.currentSearch) ||
            item.preview.includes(this.currentSearch) ||
            item.msg.includes(this.currentSearch)
        );
      }
    },
  },
  created: function () {
    this.currentlistofemail = this.listofemailinbox;
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
}
.main__body {
  display: flex;
  height: 50vh;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  margin: 0px;
}
</style>

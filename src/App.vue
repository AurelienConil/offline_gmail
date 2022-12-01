<template>
  <div id="app">
    <EmailWrite
      v-if="writingEmail"
      v-on:close-mail="closeMail"
      v-on:reduce-mail="reduceMail"
      v-on:send-mail="sendMail"
    />
    <Header 
    :msg="currentSearch"
    v-on:search="search"
    />
    <div class="main__body">
      <Sidebar
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
        v-on:leave-view="leaveView"
      />
      <EmailList
        v-else
        :listofmail="currentlistofemail"
        v-on:list-selection="mailSelected"
        v-on:element-starred="mailStarred"
        v-on:element-important="mailImportant"
        v-on:select-all="selectAll"
        v-on:delete-selected="deleteSelected"
        v-on:element-open="mailOpen"
      />
    </div>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import Sidebar from "./components/Sidebar.vue";
import EmailList from "./components/EmailList.vue";
import EmailView from "./components/EmailView.vue";
import EmailWrite from "./components/EmailWrite.vue";
import dataEmailInbox from "./emailDataInbox.json";
import dataEmailSent from "./emailDataSent.json";

export default {
  name: "App",
  components: {
    Header,
    Sidebar,
    EmailList,
    EmailView,
    EmailWrite,
  },
  data: () => ({
    readingEmail: false,
    writingEmail: false,
    viewMail: {
      author: "baba",
      object: "bibi",
      content:
        "Je cherche à me ",
    },
    currentlistofemail: [],
    currentcategoryemail : "inbox",
    currentSearch : "",
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
      this.currentlistofemail[index].read = true;
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
      this.listofemailsent.push(m);
    },
    search: function(s){
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
    deleteSelected: function () {
      this.currentlistofemail = this.currentlistofemail.filter(
        (item) => !item.selected
      );
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
      this.currentcategoryemail = "inbox"
      this.resetCurrentList();
    },
    sidebarStarred: function () {
      this.currentcategoryemail = "starred"
      this.resetCurrentList();
    },
    sidebarImportant: function () {
      this.currentcategoryemail = "important"
      this.resetCurrentList();
    },
    sidebarSent: function () {
      this.currentcategoryemail = "sent"
      this.resetCurrentList();
    },
    
    sidebarSnoozed: function () {},
    sidebarDraft: function () {},
    resetCurrentList : function(){
      switch(this.currentcategoryemail ){
        case "inbox":
          this.currentlistofemail = this.listofemailinbox;
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
      if(this.currentSearch.length>0){
              this.currentlistofemail = this.currentlistofemail.filter(
        (item) => (item.author.includes(this.currentSearch) || item.preview.includes(this.currentSearch) || item.msg.includes(this.currentSearch))
      );
      }
    }
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

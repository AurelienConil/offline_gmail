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
        :to="viewMail.to"
        :cc="viewMail.cc"
        :bcc="viewMail.bcc"
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
      const mail = this.currentlistofemail[index];
      this.viewMail.author = mail.author;
      this.viewMail.to = mail.to || [];
      this.viewMail.cc = mail.cc || [];
      this.viewMail.bcc = mail.bcc || [];
      this.viewMail.object = mail.preview;
      this.viewMail.content = mail.msg;
      this.viewMail.index = index;
      this.viewMail.time = mail.time;
      this.currentlistofemail[index].read = false; // read means unread
      this.readingEmail = true;
    },
    sendMail: function (mail) {
      let m = {
        author: mail.author,
        to: mail.to,
        cc: mail.cc,
        bcc: mail.bcc,
        msg: mail.content,
        time: "10pm",
        preview: mail.object,
        read: true,
        starred: false,
        important: false,
        spam: false
      };
      this.listofemailsent.push(m);
      this.writeMail.author = "";
      this.writeMail.object = "";
      this.writeMail.content = "";
      this.writingEmail= false;
      console.log(mail);
      // Affiche une notification personnalisée façon Gmail
      const notif = document.createElement('div');
      notif.textContent = 'GMAIL vous notifie : mail envoyé';
      notif.style.position = 'fixed';
      notif.style.top = '30px';
      notif.style.right = '70px';
      notif.style.background = '#323232';
      notif.style.color = '#fff';
      notif.style.padding = '16px 32px';
      notif.style.borderRadius = '8px';
      notif.style.boxShadow = '0 2px 8px rgba(0,0,0,0.2)';
      notif.style.fontSize = '1.1rem';
      notif.style.zIndex = '9999';
      notif.style.opacity = '0.95';
      notif.style.transition = 'opacity 0.3s';
      document.body.appendChild(notif);
      setTimeout(() => {
        notif.style.opacity = '0';
        setTimeout(() => document.body.removeChild(notif), 300);
      }, 2000);
    },
    answerMail: function (arg) {
      let aut = this.viewMail.author; // toujours l'expéditeur
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
      this.readingEmail = false;
      this.resetCurrentList();
    },
    sidebarStarred: function () {
      this.currentcategoryemail = "starred";
      this.readingEmail = false;
      this.resetCurrentList();
    },
    sidebarImportant: function () {
      this.currentcategoryemail = "important";
      this.readingEmail = false;
      this.resetCurrentList();
    },
    sidebarSent: function () {
      this.currentcategoryemail = "sent";
      this.readingEmail = false;
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

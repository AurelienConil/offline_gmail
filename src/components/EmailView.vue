<template>
  <div class="emailMain">
    <div class="emailHeader">
      <span class="material-icons" v-on:click="leaveView">arrow_back </span>
      <span
        class="material-icons"
        title="déplacer dans boite de reception"
        v-on:click="action('inbox')"
      >
        move_to_inbox</span
      >
      <span
        class="material-icons"
        title="marquer comme spam"
        v-on:click="action('spam')"
      >
        error_out</span
      >
      <span
        class="material-icons"
        title="supprimer"
        v-on:click="action('delete')"
      >
        delete
      </span>
      <span
        class="material-icons"
        title="marquer comme non lu"
        v-on:click="action('unread')"
      >
        mail
      </span>
      <span class="material-icons"> access_time</span>
      <span class="material-icons"> playlist_add_check</span>
      <span class="material-icons"> create_new_folder</span>
      <span class="material-icons" v-on:click="action('important')">
        label_outline
      </span>
      <span class="material-icons"> more_vert </span>
    </div>
    <div class="emailObject">
      {{ object }}
    </div>
    <div class="emailAuthor">
      <div class="circlePeople">&nbsp;</div>
      <div class="emailAuthor_title">
        <strong> {{ getNameAuthor() }} </strong> {{ "&lt;" + author + "&gt;" }}
        <p>A moi</p>
      </div>
        <div v-if="to && to.length">
          <span><b>À :</b> {{ to.join(', ') }}</span>
        </div>
        <div v-if="cc && cc.length">
          <span><b>CC :</b> {{ cc.join(', ') }}</span>
        </div>
        <div v-if="bcc && bcc.length">
          <span><b>CCI :</b> {{ bcc.join(', ') }}</span>
        </div>
      <div class="emailAuthor_settings">
        nov. 2022 {{ time }} ( il y a 5 jours )
        <span class="material-icons" :style="{color: isStarred ? '#FFD700' : ''}" v-on:click="toggleStar">{{ isStarred ? 'star' : 'star_border' }}</span>
        <span class="material-icons" v-on:click="answer"> arrow_back</span>
        <span class="material-icons" v-on:click="showMenu"> more_vert </span>
      </div>
    </div>
    <div class="emailView">{{ content }}</div>
  </div>
</template>

<script>
export default {
  name: "EmailView",
  props: {
    author: String,
    to: Array,
    cc: Array,
    bcc: Array,
    object: String,
    content: String,
    time: String,
    index: Number,
  },
  data() {
    return {
      selectAllCheckbox: false,
    };
  },
  computed: {
    isStarred() {
      if (typeof this.index === 'number' && this.$parent && this.$parent.currentlistofemail) {
        return this.$parent.currentlistofemail[this.index]?.starred;
      }
      return false;
    }
  },
  methods: {
    leaveView: function () {
      this.$emit("leave-view");
    },
    action: function (actionName) {
      this.$emit("action", [actionName, this.index]);
    },
    answer: function () {
      // On répond au premier destinataire du champ "to" si présent, sinon à author
      let replyTo = '';
      if (Array.isArray(this.to) && this.to.length > 0) {
        replyTo = this.to[0];
      } else if (this.author) {
        replyTo = this.author;
      }
      this.$emit("answer", [replyTo, this.object]);
    },
    getNameAuthor: function () {
      // Si plusieurs auteurs (tableau)
      const formatName = (email) => {
        if (!email || typeof email !== 'string') return '';
        let split = email.split("@");
        if (!split[0] || split[0].length === 0) return email;
        let name = split[0].split("");
        if (name.length > 0) name[0] = name[0].toUpperCase();
        name = name.join("");
        return name;
      };
      if (Array.isArray(this.author)) {
        return this.author.map(formatName).join(', ');
      } else {
        return formatName(this.author);
      }
    },
    toggleStar: function () {
      if (typeof this.index === 'number' && this.$parent && this.$parent.currentlistofemail) {
        let mail = this.$parent.currentlistofemail[this.index];
        if (mail) mail.starred = !mail.starred;
      }
    },
    showMenu: function () {
      alert('Fonctionnalité à venir : menu actions mail.');
    },
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
  width: 30%;
  margin: 0px;
  float: left;
}

.emailAuthor_settings {
  text-align: right;
  vertical-align: text-top;
  width: 60%;
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
  margin: 30px;
  text-align: left;
}
</style>

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
      <span class="material-icons" @click="toggleAccountModal" style="cursor: pointer;"> account_circle </span>
    </div>
    
    <!-- Modal utilisateur -->
    <div v-if="showAccountModal" class="account-modal" @click.self="closeAccountModal">
      <div class="account-panel">
        <div class="account-header">
          <img src="~@/assets/ericdodel.jpg" alt="Profile" class="account-avatar">
          <div class="account-info">
            <h3>Eric Dodel</h3>
            <p>eric.dodel@gmail.com</p>
          </div>
        </div>
        <div class="account-actions">
          <button class="account-button">Gérer votre compte Google</button>
          <hr class="account-divider">
          <button class="account-button">Ajouter un autre compte</button>
          <button class="account-button">Se déconnecter</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "AppHeader",
  props: {
    msg: String,
  },
  data() {
    return {
      searchMsg: "",
      showAccountModal: false,
    };
  },
  methods:{
    deleteSearch: function () {
      this.searchMsg = "";
    },
    toggleAccountModal: function () {
      this.showAccountModal = !this.showAccountModal;
    },
    closeAccountModal: function () {
      this.showAccountModal = false;
    },
  },
  mounted() {
    // Fermer la modal avec la touche Échap
    document.addEventListener('keydown', (e) => {
      if (e.key === 'Escape' && this.showAccountModal) {
        this.closeAccountModal();
      }
    });
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

/* Modal utilisateur */
.account-modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.1);
  z-index: 1000;
  display: flex;
  justify-content: flex-end;
  align-items: flex-start;
  padding-top: 70px;
  padding-right: 20px;
}

.account-panel {
  background: white;
  border-radius: 8px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
  width: 320px;
  max-width: 90vw;
  overflow: hidden;
  animation: fadeIn 0.2s ease-out;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(-10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.account-header {
  padding: 20px;
  display: flex;
  align-items: center;
  border-bottom: 1px solid #e8eaed;
}

.account-avatar {
  width: 60px;
  height: 60px;
  border-radius: 50%;
  object-fit: cover;
  margin-right: 15px;
}

.account-info h3 {
  margin: 0 0 5px 0;
  font-size: 16px;
  font-weight: 500;
  color: #202124;
}

.account-info p {
  margin: 0;
  font-size: 14px;
  color: #5f6368;
}

.account-actions {
  padding: 10px 0;
}

.account-button {
  width: 100%;
  padding: 12px 20px;
  border: none;
  background: none;
  text-align: left;
  cursor: pointer;
  font-size: 14px;
  color: #202124;
  transition: background-color 0.2s;
}

.account-button:hover {
  background-color: #f8f9fa;
}

.account-divider {
  border: none;
  border-top: 1px solid #e8eaed;
  margin: 8px 0;
}
</style>

<template>
  <div class="container">
    <div class="input-group">
      <input v-model="input" type="text" class="input" />
      <span v-if="showLoader" class="loader"></span>
      <img
        @click="showHelpers = !showHelpers"
        src="~/static/Chevron down.png"
        alt="img"
        :class="showHelpers ? 'rotaeImg' : ''"
      />
    </div>
    <div v-if="showHelpers" class="wrapperSearchBlock">
      <ul>
        <li @click="choiceUser(item)" v-for="item in info" :key="item.id">
          @ {{ item.login }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "NuxtTutorial",
  data() {
    return {
      input: "",
      showHelpers: false,
      showLoader: false,
      info: [],
    };
  },
  methods: {
    choiceUser(user) {
      this.input = user.login;
    },
  },
  watch: {
    async input() {
      this.showLoader = true;
      if (this.input !== "") {
        let response = await fetch(
          `https://api.github.com/search/users?q=${this.input}`
        );
        let resp = await response.json();
        this.info = resp.items;
        this.showLoader = false;
        this.showHelpers = true;
      } else {
        this.info = [];
        this.showHelpers = false;
        this.showLoader = false;
      }
    },
  },
};
</script>

<style scoped>
.container {
  display: flex;
  justify-content: center;
  flex-direction: column;
  position: relative;
  left: 48vh;
  top: 40vh;
}

.input-group {
  width: 280px;
  padding: 10px;
  border: double 3px transparent;
  border-radius: 4px;
  background-image: linear-gradient(white, white),
    linear-gradient(to right, #ffc700, #a5e870);
  background-origin: border-box;
  background-clip: padding-box, border-box;
  justify-content: space-between;
  display: flex;
}

.input {
  border: none;
  outline: none;
}

.rotaeImg {
  transform: rotate(180deg);
}

.wrapperSearchBlock {
  margin: -2px 0 0 0;
  width: 280px;
  border: double 3px transparent;
  border-radius: 4px;
  background-image: linear-gradient(white, white),
    linear-gradient(to right, #ffc700, #a5e870);
  background-origin: border-box;
  background-clip: padding-box, border-box;
  max-height: 127px;
  overflow: scroll;
}

.loader {
  width: 25px;
  height: 25px;
  border: 3px solid rgb(255, 251, 0);
  border-bottom-color: transparent;
  border-radius: 50% !important;
  display: inline-block;
  box-sizing: border-box;
  animation: rotation 1s linear infinite;
}

@keyframes rotation {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
</style>

<style>
ul {
  list-style: none;
  padding: 0 10px;
}

li:hover {
  background-color: #fff39a;
  list-style: none;
}

.element::-webkit-scrollbar {
  width: 0;
}

/*Убрать полосу прокрутки для всей страницы*/
::-webkit-scrollbar {
  width: 0;
}
</style>
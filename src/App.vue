<template>
  <div>
    <div class="header">
      <router-link class="router-link" :to="{path:'/'}">
        <div class="logo">
          <img
            src="./assets/cocktail.png"
            alt="logo d'un verre à cocktail rouge"
          />
          <p>IMM-IT</p>
        </div>
      </router-link>
      <div class="welcomeMessage">Bienvenue cher collaborateur !</div>
    </div>

    <router-link to="/"></router-link>
    <router-view />

    <footer>Imm-it © 2021</footer>

    <link rel="preconnect" href="https://fonts.googleapis.com" />
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
    <link
      href="https://fonts.googleapis.com/css2?family=Laila:wght@700&family=Lato:ital,wght@0,100;0,300;0,400;0,700;0,900;1,100;1,300;1,400;1,700;1,900&display=swap"
      rel="stylesheet"
    />
  </div>
</template>

<style scoped>
.logo {
  display: flex;
  align-items: center;
}

.welcomeMessage {
  display: flex;
  font-family: "Lato";
  align-items: center;
  color: #0b295d;
  font-weight: bold;
  font-size: 20px;
}

img {
  width: 50px;
}

p {
  font-family: "Laila";
  font-weight: bold;
  margin-block-end: 0;
  margin-block-start: 0;
  display: flex;
  align-items: center;
  font-size: 25px;
  color: #d41418;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 1200px;
  margin: auto;
  height: 75px;
}

footer {
    font-family: lato;
    background-color: #0B295D;
    display: flex;
    justify-content: center;
    padding: 40px;
    margin-top: 300px;
    color:white;
}

.router-link {
  text-decoration: none;
}

@media screen and (max-width: 1200px) {

    .header {
        margin: auto;
        height : 75px;
        width: 100%;  
    }


    .header .logo {
        padding-left: 50px;
    }
}

@media screen and (max-width: 650px) {

    .header {
        width: 100%;
        display:flex;
        justify-content: center;
    }

    .header .welcomeMessage {
        display: none;
    }

    .header .logo {
        padding-left: 50px;
    }
}
</style>

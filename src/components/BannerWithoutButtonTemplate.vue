<template>
  <div class="banner">
    <div class="bannerTitle">Réservez vos vacances grâce au CE</div>
  </div>
</template>

<script>

export default {
    name: "BannerWithoutButtonTemplate"

};
</script>

<style>
.banner {
    height: 50vh;
    background: url('../assets/banner.jpeg') no-repeat;
    background-size: cover;
    background-position-y: center;
    display:flex;
    justify-content: center;
    align-items: center;

}

.bannerTitle {
    color: white;
    font-family: lato;
    font-weight:500;
    font-size: 60px;
    width: 700px;
    display: flex;
    justify-content: center;
    text-align: center;
}

body {
    margin:0;
}

@media screen and (max-width: 650px) {
    .bannerTitle {
        font-size: 50px;
        width: 90%;
    }
}




</style>
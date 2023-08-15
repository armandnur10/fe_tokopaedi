<template>
  <div class="tempelate">

    <div class="container mt-5">
    <h1>WELCOME TO TOK☀️PAEDI😍</h1>
    <a class="btn btn-success mt-5 p-3" href="http://localhost:3000/gadget">get Started</a>
    </div>
    <img src="../static/images/gunung.png" class="gunung" alt="">
    <img src="../static/images/rumput.png" class="rumput" alt="">
  </div>
</template>

<script>
  export default {
    name: 'IndexPage',
  }

</script>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Tilt+Prism&display=swap');

  * {
    font-family: 'Tilt Prism', cursive;
  }

  body {
    background: #D6F5FF;
    overflow: hidden;
  }
  .tempelate{
    width: 100vw;
    height: 100vh;
  }
  .gunung{
    position: absolute;
    bottom: -100px;
    right: -250px;
    width: 800px;
  }
  .rumput{
    position: absolute;
    bottom: -50px;
    left: 0;
    height: 180px;
    z-index: 2;
  }

</style>

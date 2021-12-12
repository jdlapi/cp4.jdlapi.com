<template>
<div id="app">
  <div class="header">
    <router-link to="/">
      <div class="logo">
        <img src="/movie.png">
      </div>
    </router-link>
    <div class="title">
      <h1>Justin's Movie Reviews</h1>
    </div>
  </div>
  <div class="content">
    <router-view />
  </div>
  <div class="footer">
    <router-link to="/admin">Admin</router-link>
    <p>Justin Lapicola</p>
    <a href="https://github.com/jdlapi/cp4.jdlapi.com.git">github repository</a>
  </div>
</div>
</template>

<style>
html {
  box-sizing: border-box;
}

body {
  font-family: 'Montserrat', sans-serif;
  font-size: 16px;
  background: ##AD8680;
  padding: 0px;
  margin: 0px;
}

/* Header */

.header {
  display: flex;
  padding: 10px 100px 0px 100px;
  background-color: #8C2515;
  color: #000;
  border-style: solid;
}

.title {
  margin-top: 5px;
}

.title h1 {
  font-size: 30px;
}

.content {
  padding: 20px 100px;
  min-height: 500px;
}

/* Footer */
.footer {
  height: 50px;
  padding: 20px 100px 0px 100px;
  background: #e3e3e3;
  font-size: 12px;
  display: flex;
  justify-content: space-around;
}

.footer a {
  color: #000;
}

.footer p {
  margin: 0px;
}

h1 {
  font-size: 20px;
}

h2 {
  font-size: 14px;
}
</style>

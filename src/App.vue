<template>
  <div id="app">
    <nav id="nav" class="navbar navbar-light bg-light">
      <a class="navbar-brand" href="#"> WatchCollectors</a>
        <div class="navbar-nav">
          <a class="nav-item nav-link" href="#"> Collectors</a>
          <a class="nav-item nav-link" href="#">All Watches </a>
          <a class="nav-item nav-link" href="#"> About </a>
        </div>
    </nav>
    <router-view/>
  </div> 
</template>

<style>
#nav {
  display: flex;
  align-items: center;
}

#nav .navbar-nav {
  display: flex;
  justify-content: space-around;
  align-items: center;
  padding: 25px;
  margin-left: auto;
}

#nav .navbar-brand {
  margin: 1.5rem;
}

#nav .nav-item {
  padding: 15px;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #424446;
}
</style>

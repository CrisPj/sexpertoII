<template>
  <div>
    <nav class="navbar" role="navigation" aria-label="main navigation">
      <div class="navbar-brand">
        <a class="navbar-item" href="https://bulma.io">
          <img src="https://bulma.io/images/bulma-logo.png" alt="Bulma: a modern CSS framework based on Flexbox"
               width="112" height="28">
        </a>
        <button class="button navbar-burger">
          <span></span>
          <span></span>
          <span></span>
        </button>
      </div>

      <div class="navbar-menu" id="navMenu">
        <div class="navbar-end">
          <nuxt-link to="/" exact class="navbar-item">Home</nuxt-link>
          <nuxt-link to="/configuracion" class="navbar-item">Configuracion</nuxt-link>
          <nuxt-link to="/about" class="navbar-item">Acerca de</nuxt-link>
        </div>
      </div>
    </nav>
    <div class="container">
      <nuxt/>
    </div>
  </div>
</template>

<script>

</script>

<template>
  <div class="alerta">
    <slot></slot>
  </div>
</template>

<style scoped>
.alerta {
  padding: 1rem;
  font-weight: 900;
  text-transform: uppercase;
  font-size: 1.6rem;
  background-color: var(--blanco);
  text-align: center;
  border-left: 0.5rem solid #b91c1c;
  color: #b91c1c;
  margin-bottom: 2rem;
}
</style>

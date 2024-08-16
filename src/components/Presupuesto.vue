<script setup>
import { ref } from "vue";
import Alerta from "./Alerta.vue";

const presupuesto = ref(0);
const error = ref("");

const emit = defineEmits(["definir-presupuesto"]);

const definirPresupuesto = () => {
  if (presupuesto.value <= 0 || presupuesto.value === "") {
    error.value = "Presupuesto No Valido";

    setTimeout(() => {
      error.value = "";
    }, 3000);
    return;
  }
  emit("definir-presupuesto", presupuesto.value);
};
</script>

<template>
  <form class="presupuesto" @submit.prevent="definirPresupuesto">
    <Alerta v-if="error">
      {{ error }}
    </Alerta>

    <div class="campo">
      <label for="nuevo-presupuesto">Definir Presupuesto</label>
      <input
        type="number"
        id="nuevo-presupuesto"
        class="nuevo-presupuesto"
        placeholder="Agrega tu Presupuesto"
        v-model.number="presupuesto"
        min="0"
      />
    </div>

    <input type="submit" value="Definir Presupuesto" />
  </form>
</template>

<style scoped>
.presupuesto {
  width: 100%;
}
.campo {
  display: grid;
  gap: 2rem;
}
.presupuesto label {
  font-size: 2.2rem;
  color: var(--azul);
  font-weight: 900;
}

.presupuesto input[type="number"] {
  background-color: var(--gris-claro);
  border-radius: 1rem;
  padding: 1rem;
  border: none;
  font-size: 2.2rem;
  text-align: center;
}
.presupuesto input[type="submit"] {
  background-color: var(--azul);
  border: none;
  padding: 1rem;
  color: var(--blanco);
  text-align: center;
  width: 100%;
  margin-top: 2rem;
  font-size: 2rem;
  font-weight: 900;
  transition: all 0.3s ease;
}

.presupuesto input[type="submit"]:hover {
  background-color: #1048a4;
  cursor: pointer;
}
</style>

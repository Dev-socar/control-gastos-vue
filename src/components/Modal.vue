<script setup>
import { ref } from "vue";
import cerrarModal from "../assets/img/cerrar.svg";
import Alerta from "./Alerta.vue";

const error = ref("");

const emit = defineEmits([
  "ocultar-modal",
  "guardar-gasto",
  "eliminar-gasto",
  "update:nombre",
  "update:cantidad",
  "update:categoria",
]);

const props = defineProps({
  modal: {
    type: Object,
    required: true,
  },
  nombre: {
    type: String,
    required: true,
  },
  cantidad: {
    type: [String, Number],
    required: true,
  },
  categoria: {
    type: String,
    required: true,
  },
  disponible: {
    type: Number,
    required: true,
  },
  id: {
    type: [String, null],
    required: true,
  },
});

const cantidadAnterior = props.cantidad;

const agregarGasto = () => {
  //Validar campos
  const { nombre, cantidad, categoria, disponible, id } = props;

  if ([nombre, cantidad, categoria].includes("")) {
    error.value = "Todos los Campos son Obligatorios";
    setTimeout(() => {
      error.value = "";
    }, 3000);
    return;
  }

  //validar cantidad
  if (cantidad <= 0) {
    error.value = "Cantidad No Valida";
    setTimeout(() => {
      error.value = "";
    }, 3000);
    return;
  }

  //validar que no se gaste mas de lo disponible
  if (id) {
    //Tomar en cuenta el gasto realizado
    if (cantidad > cantidadAnterior + disponible) {
      error.value = "Has Excedido el Presupuesto";
      setTimeout(() => {
        error.value = "";
      }, 3000);
      return;
    }
  } else {
    if (cantidad > disponible) {
      error.value = "Has Excedido el Presupuesto";
      setTimeout(() => {
        error.value = "";
      }, 3000);
      return;
    }
  }

  //Emitir el gasto
  emit("guardar-gasto");
};
</script>

<template>
  <div class="modal">
    <div class="cerrar-modal">
      <img
        :src="cerrarModal"
        alt="icono cerrar modal"
        @click="$emit('ocultar-modal')"
      />
    </div>

    <div
      class="contenedor contenedor-formulario"
      :class="[modal.animar ? 'animar' : 'cerrar']"
    >
      <form class="nuevo-gasto" @submit.prevent="agregarGasto">
        <legend>{{ id ? "Modificar Gasto" : "Nuevo Gasto" }}</legend>
        <Alerta v-if="error">{{ error }}</Alerta>
        <div class="campo">
          <label for="nombre">Nombre Gasto:</label>
          <input
            type="text"
            id="nombre"
            placeholder="Agrega el Nombre del Gasto"
            :value="nombre"
            @input="$emit('update:nombre', $event.target.value)"
          />
        </div>
        <div class="campo">
          <label for="cantidad">Cantidad:</label>
          <input
            type="number"
            id="cantidad"
            placeholder="Agrega la Cantidad del Gasto, ej. 200"
            :value="cantidad"
            @input="$emit('update:cantidad', +$event.target.value)"
          />
        </div>
        <div class="campo">
          <label for="categoria">Categoria:</label>
          <select
            id="categoria"
            :value="categoria"
            @input="$emit('update:categoria', $event.target.value)"
          >
            <option value="">--Seleccione una Categoria--</option>
            <option value="ahorro">Ahorro</option>
            <option value="comida">Comida</option>
            <option value="casa">Casa</option>
            <option value="gastos">Gastos Varios</option>
            <option value="ocio">Ocio</option>
            <option value="salud">Salud</option>
            <option value="suscripciones">Suscripciones</option>
          </select>
        </div>
        <input
          type="submit"
          :value="[id ? 'Guardar Cambios' : 'Agregar Gasto']"
        />
      </form>
      <button
        v-if="id"
        type="button"
        class="btn-eliminar"
        @click="$emit('eliminar-gasto')"
      >
        Eliminar Gasto
      </button>
    </div>
  </div>
</template>

<style scoped>
.modal {
  position: absolute;
  background-color: rgb(0 0 0 / 0.9);
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
}
.cerrar-modal {
  position: absolute;
  right: 3rem;
  top: 3rem;
}
.cerrar-modal img {
  width: 3rem;
  cursor: pointer;
}
.contenedor-formulario {
  transition-property: all;
  transition-duration: 300ms;
  transition-timing-function: ease-in;
  opacity: 0;
}
.contenedor-formulario.animar {
  opacity: 1;
}
.contenedor-formulario.cerrar {
  opacity: 0;
}
.nuevo-gasto {
  margin: 10rem auto 0 auto;
  display: grid;
  gap: 2rem;
}
.nuevo-gasto legend {
  color: var(--blanco);
  font-size: 3rem;
  text-align: center;
  font-weight: 700;
}
.campo {
  display: grid;
  gap: 1rem;
}
.nuevo-gasto input,
.nuevo-gasto select {
  background-color: var(--gris-claro);
  padding: 1rem;
  border-radius: 1rem;
  border: none;
  font-size: 2rem;
}
.nuevo-gasto label {
  color: var(--blanco);
  font-size: 3rem;
}
.nuevo-gasto input[type="submit"] {
  background-color: var(--azul);
  color: var(--blanco);
  cursor: pointer;
  font-weight: 700;
}
.btn-eliminar {
  border: none;
  background-color: #ef4444;
  color: var(--blanco);
  cursor: pointer;
  padding: 1rem;
  width: 100%;
  font-weight: 700;
  font-size: 2rem;
  border-radius: 1rem;
  margin-top: 5rem;
}
</style>

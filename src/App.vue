<script setup>
import { reactive, ref, watch } from 'vue';
import Presupuesto from "./components/Presupuesto.vue"
import ControlPresupuesto from "./components/ControlPresupuesto.vue"
import iconoNuevoGasto from "./assets/img/nuevo-gasto.svg"
import Modal from "./components/Modal.vue"
import { generarId } from "./helpers"
import Gasto from "./components/Gasto.vue"

const modal = reactive({
    mostrar: false,
    animar: false,
})

const gasto = reactive({
    nombre: '',
    categoria: '',
    cantidad: '',
    id: null,
    fecha: Date.now()
})
const gastos = ref([])
const presupuesto = ref(0);
const disponible = ref(0);
const gastado = ref(0);

watch(gastos, () => {
  const totalGastado = gastos.value.reduce((total, gasto) => total + gasto.cantidad, 0)
  gastado.value = totalGastado
  disponible.value = presupuesto.value - totalGastado
}, { deep: true })

watch(modal, () => {
  if(!modal.mostrar){
    //Reiniciar modal de gasto
    reiniciarStateGasto()
    } 
}, { deep: true })

const definirPresupuesto = (cantidad) => {
  presupuesto.value = cantidad
  disponible.value = cantidad
}

const mostrarModal = () => {
  modal.mostrar = true;
  setTimeout(() => {
    modal.animar = true;
  }, 300);
}
const cerrarModal = () => {
   modal.animar = false; 
    setTimeout(() => {
    modal.mostrar = false;
    }, 300);
}

const guardarGasto = () => {
  if(gasto.id){//Es un gasto existente
    const { id } = gasto
    const i = gastos.value.findIndex(gasto => gasto.id === id)
    gastos.value[i] = {...gasto}
    
  }else{//Es un gistro nuevo
    gastos.value.push({
         ...gasto,
         id: generarId(),
     
    })
  }
    cerrarModal()
    reiniciarStateGasto()
}

//Funcion para el objeto del gasto
const reiniciarStateGasto = () => {
    Object.assign(gasto, {
        nombre: '',
        categoria: '',
        cantidad: '',
        id: null,
        fecha: Date.now()
    })
}

const seleccionarGasto = id =>{
    const gastoEditar = gastos.value.filter(gasto => gasto.id === id)[0]
    Object.assign(gasto, gastoEditar)
    mostrarModal()
}

</script>

<template>
  <div :class="{fijar: modal.mostrar }">
    <header>
      <h1>Admin Gastos</h1>
      <div class="contenedor-header contenedor sombra">
        <Presupuesto
        v-if="presupuesto === 0"
        @definir-presupuesto="definirPresupuesto"
        />
        <ControlPresupuesto v-else
        :presupuesto="presupuesto"
        :disponible="disponible"
        :gastado="gastado"
        />
      </div>
    </header>
  </div>
  <main v-if="presupuesto > 0">

    <div class="listado-gastos contenedor">
        <h2>{{ gastos.length > 0 ? 'Lista de gastos' : 'No hay gastos' }}</h2>
        <Gasto
        v-for="gasto in gastos"
        :key="gasto.id"
        :gasto="gasto"
        @seleccionar-gasto="seleccionarGasto"
        />
    </div>

    <div class="crear-gasto" @click="mostrarModal">
      <img :src="iconoNuevoGasto" alt="Icono nuevo gato">
    </div>
  </main>
  <Modal
  v-if="modal.mostrar"
  @cerrar-modal="cerrarModal"
  @guardar-gasto="guardarGasto"
  :modal="modal"
  :disponible="disponible"
  :id="gasto.id"
  v-model:nombre="gasto.nombre"
  v-model:categoria="gasto.categoria"
  v-model:cantidad="gasto.cantidad"
  />
</template>

<style> 
/* Estilos Globales en App.vue */
  :root {
    --azul: #3b82f6;
    --blanco: #fff;
    --gris-claro: #f5f5f5;
    --gris: #94a3b8;
    --gris-oscuro: #64748b;
    --negro: #1f2937;
  }
  html{
    font-size: 62.5%;
    box-sizing: border-box;
  }
  *, *::before, *::after {
    box-sizing: inherit;
  }
  body{
    font-size: 1.6rem;
    font-family: 'Lato', sans-serif;
    background-color: var(--gris-claro);
  }
  h1{
    font-size: 4rem;
  }
  h2{
    font-size: 3rem;
  }
  .fijar{
    overflow: hidden;
    position: fixed;
    height: 100vh;
    width: 100vh;
  }
  header{
    background-color: var(--azul);
  }
  header h1{
    padding: 3rem 0;
    margin: 0;
    color: var(--blanco);
    text-align: center;
  }
  .contenedor{
    width: 90%;
   max-width:80rem;
   margin: 0 auto;
   
  }
  .contenedor-header{
    margin-top: -5rem;
    transform: translateY(5rem);
    padding: 5rem;
  }
  .sombra{
    box-shadow: 0px 10px 15px -3px rgba(0, 0, 0, 0.1);
    background-color: var(--blanco);
    border-radius: 1.2rem;
    padding: 5rem;
  }
  .crear-gasto{
    position: fixed;
    bottom: 5rem;
    right: 5rem;

  }
  .crear-gasto img{
    width: 5rem;
    margin: 5rem auto;
  }
  .crear-gasto:hover{
    cursor: pointer;
    opacity: .8;
  }
  .listado-gastos{
    margin: 10rem;
  }
  .listado-gastos h2{
    font-weight: 900;
    color: var(--gris-oscuro);
  }
    
</style>

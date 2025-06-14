<script setup>
import { ref } from 'vue';
import Alerta from './Alerta.vue' //Reutilizamos el componente Alerta
import cerrarModal from '../assets/img/cerrar.svg'
const error = ref('')

const emit = defineEmits(['cerrar-modal', 'guardar-gasto', 'update:nombre', 'update:categoria', 'update:cantidad'])
const props = defineProps({
    modal: {
        type: Object,
        required: true
    },
    nombre: {
        type: String,
        required: true
    },
    categoria: {
        type: String,
        required: true
    },
    cantidad: {
        type: [Number, String],
        required: true
    },
    disponible: {
        type: Number,
        required: true
    }
})
const agregarGasto = () => {
    //validar gasto
    const { nombre, categoria, cantidad, disponible } = props //Aplicamos desestructuracion
    if([nombre, categoria, cantidad].includes('')){//Metemos en un areglo para usar el includes
        error.value = 'Todos los campos son obligatorios'
        setTimeout(() => {
            error.value = ''
        }, 3000)
        return
    }
    //validar cantidad
    if(cantidad <= 0){
        error.value = 'La cantidad no valida'
        setTimeout(() => {
            error.value = ''
        }, 3000)
        return
    }
    //Validar que el usuario no pueda gastar mas de lo disponible
    if(cantidad > disponible){
        error.value = 'Excedes el presupuesto'
        setTimeout(() => {
            error.value = ''
        }, 3000)
        return
    }
    emit('guardar-gasto', {nombre, categoria, cantidad})
}
</script>
<template>
  <div class="modal">
    <div class="cerrar-modal">
        <img :src="cerrarModal" alt="Cerrar modal" @click="$emit('cerrar-modal')">
    </div>
    <div class="contenedor contenedor-formulario"
    :class="[modal.animar ? 'animar' : 'cerrar']">
        <form class="nuevo-gasto"
        @submit.prevent="agregarGasto"
        >
            <legend>Añadir gasto</legend>
            <Alerta v-if="error" >{{ error }}</Alerta>
            <div class="campo">
                <label for="nombre">Nombre gasto:</label>
                <input type="text" id="nombre" placeholder="Añade el nombre del gasto" :value="nombre" @input="$emit('update:nombre', $event.target.value)">
            </div>
            <div class="campo">
                <label for="cantidad">Cantidad:</label>
                <input type="number" id="cantidad" placeholder="Añade la cantidad" :value="cantidad" @input="$emit('update:cantidad', +$event.target.value)">
            </div>
            <div class="campo">
                <label for="categoria">Categoria:</label>
                <select name="" id="categoria" :value="categoria" @input="$emit('update:categoria', $event.target.value)">
                    <option value="">Seleccione una categoria</option>
                    <option value="ahorro">ahorro</option>
                    <option value="comida">comida</option>
                    <option value="casa">casa</option>
                    <option value="salud">salud</option>
                    <option value="gastos varios">gastos varios</option>
                    <option value="ocio">ocio</option>
                    <option value="suscripciones">suscripciones</option>
                </select>
            </div>
            <input type="submit" value="Agregar gasto">
        </form>
    </div>
  </div>
</template>
<style scoped>
.modal{
    position: absolute;
    background-color: rgba(0, 0, 0, 0.9);
    color: var(--blanco);
    top: 0;
    left: 0;
    bottom: 0;
right: 0;
}
.cerrar-modal{
    position: absolute;
    top: 2rem;
    right: 2rem;
    width: 3rem;
    cursor: pointer;
}
.contenedor-formulario{
    transition: opacity 300ms ease-in;
    opacity: 0;
}
.contenedor-formulario.animar{
    opacity: 1;
}
.contenedor-formulario.cerrar{
    opacity: 0;
}

.nuevo-gasto{
    margin: 10rem auto 0 auto;
    display: grid;
    gap: 2rem;
}
.nuevo-gasto legend{
    font-size: 2rem;
    text-align: center;
    font-weight: 700;
}
.campo{
    display: grid;
    gap: 2rem;
}
.nuevo-gasto input,
.nuevo-gasto select{
    background-color: var(--gris-claro);
    border-radius: 1rem;
    padding: 1rem;
    border: none;
    font-size: 2rem;
}
.nuevo-gasto label{
    color: var(--blanco);
    font-size: 2rem;
}
.nuevo-gasto input[type="submit"]{
    background-color: var(--azul);
    border: none;
    padding: 1rem;
    text-align: center;
    margin-top: 2rem;
    font-size: 2rem;
    color: var(--blanco);
    font-weight: 900;
    width: 100%;
    border-radius: 1rem;
    cursor: pointer;
    transition: all .3s ease;
}
.nuevo-gasto input[type="submit"]:hover{
    background-color: #2563eb;
    cursor: pointer;
}




</style>
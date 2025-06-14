<script setup>
import { ref, computed } from 'vue';
import Alerta from './Alerta.vue' //Reutilizamos el componente Alerta
import cerrarModal from '../assets/img/cerrar.svg'
const error = ref('')

const emit = defineEmits(['cerrar-modal', 'guardar-gasto', 'update:nombre', 'update:categoria', 'update:cantidad', 'eliminar-gasto'])
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
    },
    id: {
        type: [String, null],
        required: true
    }
})
const old = props.cantidad //Tomamos el valor original de la cantidad para posteriores validarlo
const agregarGasto = () => {
    //validar gasto
    const { nombre, categoria, cantidad, disponible, id } = props //Aplicamos desestructuracion
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
    if(id){
        //Tomar en cuenta el gasto ya realizado
        if(cantidad > disponible + old){
            error.value = 'Excedes el presupuesto'
            setTimeout(() => {
                error.value = ''
            }, 3000)
            return
        }
    }else{ //Si no es un gasto existente 
         if(cantidad > disponible){
        error.value = 'Excedes el presupuesto'
        setTimeout(() => {
            error.value = ''
        }, 3000)
        return
    } 
    }
  
    emit('guardar-gasto', {nombre, categoria, cantidad})
}
const isEditing = computed(()=>{
    return props.id //Esto es otra forma mas descritiva hacer que tenga una idea apoximada de lo que hace por si otro dev lee este codigo
})
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
            <legend>{{ isEditing ? 'Editar gasto' : 'Agregar gasto' }}</legend>
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
            <input type="submit" value="Agregar gasto" v-if="id === null">
            <!-- O input submit :value='id === null ? 'Agregar gasto' : 'Guardar cambios' Esta es una forma de hacerlo -->
            <input type="submit" value="Guardar cambios" v-else>
        </form>
        <button type="button" v-if="isEditing" @click="$emit('eliminar-gasto', id)" class="btn-eliminar">Eliminar gasto</button>
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
.btn-eliminar{
    background-color: #dc2626;
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
.btn-eliminar:hover{
    background-color: #b91c1c;
    cursor: pointer;
}


</style>
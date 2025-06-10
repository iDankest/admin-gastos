<script setup>
import cerrarModal from '../assets/img/cerrar.svg'

const emit = defineEmits(['cerrar-modal'])

</script>
<template>
  <div class="modal">
    <h2>Agregar nuevo gasto</h2>
    <div class="cerrar-modal">
        <img :src="cerrarModal" alt="Cerrar modal" @click="$emit('cerrar-modal')">
    </div>
    <div class="contenedor">
        <form class="nuevo-gasto">
            <legend>Añadir gasto</legend>
            <div class="campo">
                <label for="nombre">Nombre gasto:</label>
                <input type="text" id="nombre" placeholder="Añade el nombre del gasto">
            </div>
            <div class="campo">
                <label for="cantidad">Cantidad:</label>
                <input type="number" id="cantidad" placeholder="Añade la cantidad">
            </div>
            <div class="campo">
                <label for="categoria">Categoria:</label>
                <select name="" id="categoria">
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
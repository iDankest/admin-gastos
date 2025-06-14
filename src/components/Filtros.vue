<script setup>
defineProps(['filtro'])
defineEmits(['update:filtro'])
</script>


<template>
  <div class="filtros sombra contenedor">
    <form >
        <div class="campo">
            <label for="filtro">Filtrar por categoria:</label>
            <select id="filtro" :value="filtro" @input="$emit('update:filtro', $event.target.value)">
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
    </form>
  </div>
</template>



<style scoped>
    .filtros{
        margin-top: 10rem;
    }
    .filtros .campo{
        display: flex;
        gap: 2rem;
        align-items: center;
    }
    .filtros label{
        font-size: 3rem;
        font-weight: 900;
        color: var(--gris-oscuro);
    }
    .filtros select{
        flex: 1;
        padding: 1rem;
        border: none;
        background-color: var(--gris-claro);
        border-radius: 1rem;
        text-align: center; 
        font-size: 1.6rem;
        
    }
</style>
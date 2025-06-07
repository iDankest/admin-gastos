<script setup>
import { ref } from 'vue';

const presupuesto = ref(0);

</script>
<template>
    <form 
    class="presupuesto"
    >
        <div class="campo">
            <label for="">Definir presupuesto</label>
            <input type="number"
                id="nuevo-presupuesto"
                class="nuevo-presupuesto"
                placeholder="Añade tu presupuesto"
                v-model="presupuesto"
            />
        </div>
        <input type="submit" value="Definir presupuesto">
    </form>
</template>



<style scoped>
.presupuesto{
    width: 100%;
}
.campo{
    display: grid;
    gap: 2rem;
}
.presupuesto label{
    font-size: 2rem;
    color: var(--azul);
    font-weight: 900;
    text-align: center;
}
.presupuesto input[type="number"]{
    background-color: var(--gris-claro);
    border-radius: 1rem;
    padding: 1rem;
    border: none;
    font-size: 2.2rem;
    text-align: center;
}
.presupuesto input[type="submit"]{
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
.presupuesto input[type="submit"]:hover{
    background-color: #2563eb;
    cursor: pointer;
}

</style>

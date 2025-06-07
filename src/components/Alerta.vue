<template>
    <div class="alerta">
        <slot></slot> <!-- Slot -->
    </div>
</template>
<style scoped>
.alerta{
    background-color: var(--blanco);
    color: #b91c1c;
    padding: 2rem;
    border-radius: 1rem;
    margin: 2rem 0;
    text-align: center;
    font-weight: 900;
    text-transform: uppercase;
    font-size: 2rem;
    text-align: center;
    border-left: .5rem solid #b91c1c;
}
</style>
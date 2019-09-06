<template>
    <div class="cuadro cuadroInicial">
    <span :class="info.claseValor">{{info.valor}}</span>
    </div>
</template>

<!-- Comunicamos al padre de la informacion del valor de la generacion de minas para que se aplique la logica --> 
<script>
export default {
    props:['info']
}
</script>
<!-- Comment --> 
<style>

.cuadro{
    display:grid;
    justify-items:center;
    align-items:center;
}

.cuadroInicial{
    width:20px;
    height:20px;
    background-color: rgb(8, 34, 34);
    border-top-color:blanchedalmond;
    border-left-color: aliceblue;
    border-right-color: silver;
    border-bottom-color: silver;
    border-style:solid;
    border-width:3px;
    cursor:pointer;/*cursor de manito*/
}
</style>
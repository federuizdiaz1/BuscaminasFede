<template>
    <div
    @mousedown.right="bandera" 
    @contextmenu.prevent
    @mouseup.left="activar"
    class="cuadro" :class="info.inicial ? 'cuadroInicial': 'cuadro-vacio'">
    <span :class="clase">{{valor}}</span>
    </div>
</template>

<!-- Comunicamos al padre de la informacion del valor de la generacion de minas para que se aplique la logica --> 
<script>
export default {
    props:['info'],
    computed: {
        valor(){
            if(this.info.inicial){
                if(this.info.bandera){
                    return '🚩'
                }
                else{
                    return ''
                }
            }
            else{
                return this.info.valor
            }
        },
        clase(){
            if(this.info.inicial){
                if(this.info.bandera){
                    return 'bandera'
                }
                else{
                    return ''
                }
            }
            else{
                return this.info.claseValor
            }
        }
    },
    methods:{
        bandera(){
            if(this.info.inicial){
                this.info.bandera = !this.info.bandera
                this.$emit('onCambiarMinasRestantes', this.info.bandera ? -1 : 1)
            }
        },
        activar(){
            this.$emit('onActivar',this.info)
        }
    }
    
}
</script>
<!-- Comment --> 
<style>
.bandera{
   font-size: 12px; 
}
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
.cuadro-vacio{
    width: 25px;
    height: 25px;
    background-color:rgb(8, 34, 34);
    border-right-color: aliceblue;
    border-bottom-color: aqua;
    border-top-width: 0;
    border-left-width: 0;
    border-bottom-width: 1px;
    border-right-width: 1px;
    border-style: solid;
    cursor: default;


}
</style>
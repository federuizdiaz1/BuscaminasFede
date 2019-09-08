<template>
    <div class="tablero">  <!-- definimos los elementos dentro de tablero: niveles -->
        <div class="niveles">
        <span>Nivel:
        </span>
        <span @click="seleccionarNivel(1)" class="nivel" :class="{'nivel-seleccionado':nivelActual.nivel == 1}">1</span>
        <span @click="seleccionarNivel(2)" class="nivel"  :class="{'nivel-seleccionado':nivelActual.nivel == 2}">2</span>
        <span @click="seleccionarNivel(3)" class="nivel" :class="{'nivel-seleccionado':nivelActual.nivel == 3}">3</span>
        </div>
        <div class="panel"> <!-- elemento panel -->
        <div class="marcador minas-restantes"> 999</div>
       
        
        <div class="cara">
            <span style='font-size:30px;'>&#x1F920;</span></div>
        <div class="marcador segundos"> 999</div>
        
       
        </div>
        <div class="matriz"> <!-- elemento matriz -->
            <!-- pintar el objeto cuadro en la matriz --> 
            <cuadro :info="item" v-for="(item, index) in cuadros" :key="index" :style="'grid-row: ' + item.fila+';grid-column:'+item.columna+';'" />
     </div>
    </div>
</template>
<script>/* Importamos el componente 'Cuadro' */
import Cuadro from './Cuadro.vue'
export default {
    components: {Cuadro},
    data(){
        return{
            cuadros:[] /** iniciamos cuadro y cargamos con datos*/,
            colores:['','uno','dos','tres','cuatro','cinco','seis','siete','ocho'] ,
            nivelPrincipiante: {
                nivel: 1,
                filas: 9,
                columnas: 9,
                minas: 10
            },
            nivelIntermedio: {
                nivel: 2,
                filas: 16,
                columnas: 16,
                minas: 40
            },
            nivelExperto: {
                nivel: 3,
                filas: 16,
                columnas: 30,
                minas: 30
            },
            nivelActual: null,
            minas:[]
    } 
    }, /* created se invoca cuando la parte inteligente del componente está lista(Es un estado) */
    created() {
        this.nivelActual = this.nivelPrincipiante
        this.iniciarNivel() 
    },
    methods: {
                /*seleccionar el nivel*/
        seleccionarNivel(nivel){
        if (this.nivelActual.nivel ==nivel){return}
        if(nivel==1){this.nivelActual = this.nivelPrincipiante} 
        else if(nivel==2){this.nivelActual=this.nivelIntermedio}
        else (this.nivelActual=this.nivelExperto)
        this.iniciarNivel()
        },
        iniciarNivel(){
            let filas = this.nivelActual.filas
            let columnas = this.nivelActual.columnas
            let totalCuadros = filas * columnas

            this.cuadros = [] /* iniciamos cuadros(de nuevo) ,vacio ya que contiene informacion*/
            let indices = []
            for (let i = 0 ; i < totalCuadros; i++){
                let cuadro = {
                    valor: "",
                    fila: Math.floor(i/columnas)+1, /** math.floor corta los decimales y deja los enteros */
                    columna: (i % columnas)+1,
                    vecinos: [],
                    claseValor:''
                }
          /*  console.log(i,cuadro.fila,cuadro.columna)
          */    this.cuadros.push(cuadro)
                indices.push(i)
            }
                /*generamos las minas aleatoriamente* */
            for(let i=0 ;i < this.nivelActual.minas; i++){
                let posicion=Math.floor(Math.random() * (indices.length - 1))
                let indice = indices[posicion]
                this.cuadros[indice].valor = "💣"
    
                indices.splice(posicion, 1)
            }

            for(let i=0 ;i <totalCuadros; i++){
                let cuadro = this.cuadros[i]
                if(cuadro.columna==1){
                    if(cuadro.fila==1){
                        cuadro.vecinos.push(i+1)
                        cuadro.vecinos.push(i+columnas)
                        cuadro.vecinos.push(i+columnas+1)

                    }
                    else if(cuadro.fila==filas){
                        cuadro.vecinos.push(i+1)
                        cuadro.vecinos.push(i-columnas)
                        cuadro.vecinos.push (i-columnas+1)
                    }
                    else{
                        cuadro.vecinos.push(i+1)
                        cuadro.vecinos.push(i+columnas)
                        cuadro.vecinos.push(i+columnas+1)
                        cuadro.vecinos.push(i-columnas)
                        cuadro.vecinos.push(i-columnas+1)
                    }
                }
                else if(cuadro.columna==columnas){
                    if(cuadro.fila==1){
                        cuadro.vecinos.push(i-1)
                        cuadro.vecinos.push(i+columnas)
                        cuadro.vecinos.push(i+columnas-1)
                    }
                    else if(cuadro.fila==filas){
                        cuadro.vecinos.push(i-1)
                        cuadro.vecinos.push(i-columnas)
                        cuadro.vecinos.push(i-columnas-1) 
                    }
                    else{
                        cuadro.vecinos.push(i-1)
                        cuadro.vecinos.push(i+columnas)
                        cuadro.vecinos.push(i+columnas-1)
                        cuadro.vecinos.push(i-columnas)
                        cuadro.vecinos.push(i-columnas-1)
                    }
                }
                else{
                    if(cuadro.fila==1){
                        cuadro.vecinos.push(i-1)
                        cuadro.vecinos.push(i+1)
                        cuadro.vecinos.push(i+columnas-1)
                        cuadro.vecinos.push(i+columnas)
                        cuadro.vecinos.push(i+columnas+1)
                    }
                    else if(cuadro.fila==filas){ 
                        cuadro.vecinos.push(i-1)
                        cuadro.vecinos.push(i+1)
                        cuadro.vecinos.push(i+columnas-1)
                        cuadro.vecinos.push(i+columnas)
                        cuadro.vecinos.push(i+columnas+1)
                    }else{
                        cuadro.vecinos.push(i-1)
                        cuadro.vecinos.push(i+1)
                        cuadro.vecinos.push(i+columnas-1)
                        cuadro.vecinos.push(i+columnas)
                        cuadro.vecinos.push(i+columnas+1)
                        cuadro.vecinos.push(i-columnas-1)
                        cuadro.vecinos.push(i-columnas)
                        cuadro.vecinos.push(i-columnas+1) 
                    }
                }
                if(cuadro.valor !="💣"){
                    let minas = cuadro.vecinos.filter(v=>this.cuadros[v].valor=="💣").length /*Operacion de retorno de un arreglo con todos los vecinos que sean minas(cantidad) */ 
                    if(minas>0){
                        cuadro.valor=minas
                        cuadro.claseValor=this.colores[minas]
                    }
                }
            }
        }
    }
}
</script>
<!-- Definimos los estilos de cada elemento en la tabla --> 
<style>
    @import url('<link href="https://fonts.googleapis.com/css?family=Roboto&display=swap" rel="stylesheet">');
/*Especifica una familia de fuentes para nuestro html completo*/
    .html{
       font-family: 'Roboto', sans-serif; 
    }
    .uno{color:teal;}
    .dos{color:thistle;}
    .tres{color:tomato;}
    .cuatro{color:blue;}
    .cinco{color:violet;}
    .seis{color:yellowgreen;}
    .siete{color:brown;}
    .ocho{color:chartreuse;}

    .tablero{
        display: grid;
        justify-content: center;
        background-color: cornflowerblue;
        padding: 10px;
    }
    .niveles{
        display: grid;
        grid-auto-flow: column;
        grid-gap: 10px;
        font-size:24px;
        padding: 5px;
        justify-content: start;
        align-items:center;

    }
    .nivel{
        width: 32px;
        height: 32px;
        color:gray;
        text-align: center;
        vertical-align: center;
        cursor:pointer;
    }

    .nivel-seleccionado{
        color:yellowgreen !important;
        background-color: aqua;
        border-style:  solid;
        border-width: 2px;
        border-radius:50%;
        cursor:default;
    }
    .panel{
        display: grid;
        grid-auto-flow: column;
        font-size: 30px;

    }
    .marcador{
         background-color: black;
         color:red;
         height: 40px;
    }
    .minas-restantes{
        justify-self: start;
    }
    .cara{
        display: grid;
        justify-content: center;
        align-items: center;
        justify-self: center;
        width: 40px;
        height: 40px;
    }
    .segundos{ 
justify-self: end;
    }
    .matriz{
        display: grid;
        background-color:darkslategray;
       
    }
</style>
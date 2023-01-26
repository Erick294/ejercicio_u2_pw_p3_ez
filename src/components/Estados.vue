<template>
  <div id="lista_estados">
    <ul>
        <li v-for:="element in listaEstados">{{element}}</li>
    </ul>
  </div>

  <input v-on:keyup.enter="buscarEstado()" v-model="codigo" type="text" placeholder="Ingrese las iniciales del estado">

  <div v-if="mostrar" id="resultados">
    <label for="positivos">Casos positivos:</label>
    <input id="positivos" type="text" value={{ positivos }}>

    <label for="resultadosT">Resultados totales:</label>
    <input id="resultadosT" type="text" value={{ resultadosTotales }}>

    <label for="hospitalizados">Hospitalizados:</label>
    <input id="hospitalizados" type="text" value={{ hospitalizados }}>

    <label for="muertos">Casos positivos:</label>
    <input id="muertos" type="text" value={{ muertos }}>

    <label for="totalViral">Casos positivos:</label>
    <input id="totalViral" type="text" value={{ totalViral }}>

    <label for="incrementoMuertos">Casos positivos:</label>
    <input id="incrementoMuertos" type="text" value={{ incrementoMuertos }}>
 
  </div>
</template>

<script>
export default {
    data(){
        return{
            listaEstados:[],
            listaDatosEstados:[],
            codigo: "",
            resultadosTotales: null,
            hospitalizados: null,
            muertos: null,
            totalViral: null,
            incrementoMuertes: null,
            mostrar: false
        }
    },
    mounted(){
        this.obtenerData();
        console.log("Mi componente se monto")
    },
    methods:{
        async obtenerApi(){
            const data = await fetch(`https://api.covidtracking.com/v1/states/current.json`).then((r) => r.json());
            return data;
        },
        async obtenerData(){
            const data = await this.obtenerApi()
            for(let i=0; i<55; i++){
                const datos = data[i]
                const {state, positive, totalTestResults, hospitalizedCurrently, death,
                                                        totalTestViral, deathIncrease} = datos
                const objetoEstado = {
                    state: state,
                    positivos: positive,
                    totalTestResults: totalTestResults,
                    hospitalizedCurrently: hospitalizedCurrently,
                    death: death,
                    totalTestViral:totalTestViral,
                    deathIncrease:deathIncrease
                }                                      
                this.listaEstados.unshift(state);
                this.listaDatosEstados.unshift(objetoEstado);
            }
        },
        async buscarEstado(){
            console.log(this.listaDatosEstados)
            for (let i=0; i<55; i++){
                const aux = this.listaDatosEstados[i]
                if(this.codigo.includes(aux.state)){
                    this.mostrar = true
                    this.resultadosTotales = aux.totalTestResults
                    this.hospitalizados = aux.hospitalizedCurrently
                    this.muertos = aux.death
                    this.totalViral = aux.totalTestViral
                    this.incrementoMuertes = aux.deathIncrease
                }else{
                    console.log(aux.state)
                }
            }
                 
        }      
    },
    renderTracked(){
        console.log("renderTracked")
    },
    renderTriggered(){
        console.log("renderTriggered")
    },unmounted(){
        console.log("unmounted")
    },
    created(){
        console.log("created")
    },
    beforeCreate(){
        console.log("beforeCreated")
    },
    beforeMount(){
        console.log("beforeMount")
    },
    beforeUpdate(){
        console.log("beforeUpdate")
    },
    updated(){
        console.log("updated")
    }

}
</script>

<style>
template{
    text-align: center;
}

#lista_estados{
    height: 455px;
    width: 100px;
}

ul{
    column-count: 4;
    height: 455px;
    width: 1300px;
}

li{
    padding: 5px;
}

</style>
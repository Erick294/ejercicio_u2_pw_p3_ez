<template>
  <div id="lista_estados">
    <li>
        <ul v-for:="element in 55">{{element.nombre}}</ul>
    </li>
  </div>

  <input v-on:keyup.enter="obtenerResultados()" type="text" placeholder="Ingrese las iniciales del estado">

  <div v-if="mostrar" id="resultados">
    <label for="positivos">Casos positivos:</label>
    <input id="positivos" type="text" placeholder={{ positivos }}>

    <label for="resultadosT">Resultados totales:</label>
    <input id="resultadosT" type="text" placeholder={{ resultadosTotales }}>

    <label for="hospitalizados">Hospitalizados:</label>
    <input id="hospitalizados" type="text" placeholder={{ hospitalizados }}>

    <label for="muertos">Casos positivos:</label>
    <input id="muertos" type="text" placeholder={{ muertos }}>

    <label for="totalViral">Casos positivos:</label>
    <input id="totalViral" type="text" placeholder={{ totalViral }}>

    <label for="incrementoMuertos">Casos positivos:</label>
    <input id="incrementoMuertos" type="text" placeholder={{ incrementoMuertos }}>
 
  </div>
</template>

<script>
export default {
    data(){
        return{
            listaEstados:[],
            codigo: "CA",
            resultadosTotales: null,
            hospitalizados: null,
            muertos: null,
            totalViral: null,
            incrementoMuertes: null,
            mostrar: false
        }
    },
    mounted:{
        async obtenerApi(){
            const data = await fetch(`https://api.covidtracking.com/v1/states/current.json`).then((r) => r.json());
            return data;
        }
    },
    methods:{
        async obtenerApiCodigo(codigo){
            const data = await fetch(`https://api.covidtracking.com/v1/states/${codigo}/current.json`).then((r) => r.json());
            return data;
        },
        async obtenerData(){
            const data = await obtenerApi()
            for(let i=0; i<55; i++){
                this.listaEstados.unshift(data[i])
            }
        },
        async construirObjetoEstado(codigo){
                const {state, positive, totalTestResults, hospitalizedCurrently, death,
                            totalTestViral, deathIncrease} = await this.obtenerApiCodigo(codigo);

                for(let i=0; i<55; i++){
                    const objetoEstado = {
                        nombre: state,
                        positivos: positive,
                        resultadosTotales: totalTestResults,
                        hospitalizados: hospitalizedCurrently,
                        muertos: death,
                        totalViral: totalTestViral,
                        incrementoMuertes: deathIncrease
                    };
                    return objetoEstado;  
                }  
                  
            },
            obtenerResultados(){
                const data = this.construirObjetoEstado(this.codigo.toLowerCase());
                this.mostrar = true
                this.positivos = data.positivos
                this.resultadosTotales = data.resultadosTotales
                this.hospitalizados = data.hospitalizados
                this.muertos = data.muertos
                this.totalViral = data.totalViral
                this.incrementoMuertes = data.incrementoMuertes
            }
    }
}
</script>

<style>
template{
    text-align: center;
}
</style>
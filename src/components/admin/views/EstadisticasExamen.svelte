<script>
  //importamos el componente de estadisticas
  import Resultados from "../smallcomponents/LLamadaApiResultados.svelte";
  import Icfes from "../smallcomponents/LLamadaApiSimulacros.svelte";
  import Final from "../smallcomponents/GraficoResultadosFinales.svelte";

  let codigoEstudiante = "";
  let isInputValid = false;
  let consultarResultados = false;
  let codigoCorrecto = "";
  let mostrarUltimoGrafico = false;

  const validarInput = () => {
    isInputValid = /^\d+$/.test(codigoEstudiante);
  };

  //funcion para el manejo del boton
  const consultar = () => {
    validarInput();
    if (isInputValid) {
      consultarResultados = true;
      codigoCorrecto = codigoEstudiante;
    }
    mostrarUltimoGrafico = false;
  };

  //función para resivir los datos enviados desde los componentes
  let resultados = [];
  let resultadosicfes = [];
  const recibirDatosIcfes = (event) => {
    resultadosicfes = event.detail;
    if(resultadosicfes != null){
      calcularDatosFinales();
    }
    //console.log(event.detail);
  };
  const recibirDatosResultados = (event) => {
    resultados = event.detail;
    //comprobamos si hay o no datos 
    if(resultados!= null){
      calcularDatosFinales();
    }
  };

  let resultadosFinales = [];

  const calcularDatosFinales = () =>{
    if(resultados && resultadosicfes.length > 0){
     
      calcularPromedioAcertadoSimulacros();
      calcularPromedioIcfes();
      calcularFinales();
      

    }
  }

let promedioAcertadoSimulacros ={}
const calcularPromedioAcertadoSimulacros = () =>{
  let promedioLectura = resultados.promedioLectura /3;
  let promedioMatematicas = resultados.promedioRazonamiento /3;
  let promedioSociales = resultados.promedioCiudadanas /3;
  let promedioIngles = resultados.promedioIngles /3;
  let promedioNaturales = resultados.promedioComunicacion /3;
  promedioAcertadoSimulacros = {
    promedioLectura,
    promedioMatematicas,
    promedioSociales,
    promedioIngles,
    promedioNaturales,
  }
  //console.log(promedioAcertadoSimulacros);
}

let promedioAcertadoIcfes ={}
const calcularPromedioIcfes = () =>{
  let promedioLectura = 0;
  let promedioMatematicas = 0;
  let promedioSociales = 0;
  let promedioIngles = 0;
  let promedioNaturales = 0;

  resultadosicfes.forEach((resultado) => {
    promedioLectura += resultado.lectura;
    promedioMatematicas += resultado.matematicas;
    promedioSociales += resultado.sociales;
    promedioIngles += resultado.ingles;
    promedioNaturales += resultado.naturales;
  });
  promedioAcertadoIcfes = {
    promedioLectura,
    promedioMatematicas,
    promedioSociales,
    promedioIngles,
    promedioNaturales,
  }
  //console.log(promedioAcertadoIcfes);
}

const calcularFinales = () =>{
  let PromedioLectura = promedioAcertadoSimulacros.promedioLectura - promedioAcertadoIcfes.promedioLectura;
  let PromedioMatematicas = promedioAcertadoSimulacros.promedioMatematicas - promedioAcertadoIcfes.promedioMatematicas;
  let PromedioSociales = promedioAcertadoSimulacros.promedioSociales - promedioAcertadoIcfes.promedioSociales;
  let PromedioIngles = promedioAcertadoSimulacros.promedioIngles - promedioAcertadoIcfes.promedioIngles;
  let PromedioNaturales = promedioAcertadoSimulacros.promedioNaturales - promedioAcertadoIcfes.promedioNaturales;
  resultadosFinales = {
    PromedioLectura,
    PromedioMatematicas,
    PromedioSociales,
    PromedioIngles,
    PromedioNaturales,
  }
  mostrarUltimoGrafico = true;
  console.log(resultadosFinales);
}


</script>

<body>
  <div class="card text-center">
    <div class="container mt-3">
      <div class="row justify-content-center">
        <div class="col-md-6">
          <div class="form-group">
            <label for="inputCodigoEstudiante">Código del Estudiante</label>
            <input
              id="inputCodigoEstudiante"
              type="text"
              class="form-control {isInputValid ? 'is-valid' : 'is-invalid'}"
              placeholder="Ingrese el código del estudiante"
              bind:value={codigoEstudiante}
              on:input={validarInput}
            />
            {#if isInputValid}
              <div class="valid-feedback">¡Todo correcto!</div>
            {:else}
              <div class="invalid-feedback">
                Por favor, ingrese solo números.
              </div>
            {/if}
          </div>
          <button
            class="btn btn-primary mt-2"
            disabled={!isInputValid}
            on:click={consultar}>Consultar</button
          >
        </div>
      </div>
    </div>
  </div>

  <div style="height: 40px;"></div>
  <!--SI el imput es correcto creamos el componente-->
  {#if consultarResultados}
    <Resultados codigo={codigoCorrecto} on:enviarDatos={recibirDatosResultados}/>
    <Icfes codigo={codigoCorrecto} on:enviarDatos={recibirDatosIcfes} />
  {/if}

  {#if mostrarUltimoGrafico}
      <Final resultadosFinales={resultadosFinales}/>
  {/if}
</body>

<style>
  .container {
    padding-top: 50px;
  }

  .card {
    background-color: rgba(255, 255, 255, 0.6);
  }

  
</style>

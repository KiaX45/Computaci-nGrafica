<script>
  //importamos el componente de estadisticas
  import Resultados from "../smallcomponents/LLamadaApiResultados.svelte";
  import Icfes from "../smallcomponents/LLamadaApiSimulacros.svelte";

  let codigoEstudiante = "";
  let isInputValid = false;
  let consultarResultados = false;
  let codigoCorrecto = "";

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
  };
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
    <Resultados codigo={codigoCorrecto} />
    <Icfes codigo={codigoCorrecto} />
  {/if}
</body>

<style>
  .container {
    padding-top: 50px;
  }

  body {
    /* Cambia la URL a la ruta correcta de tu imagen */
    background-image: url("https://situr.narino.gov.co/storage/Clientes/situr_narino/principal/imagenes/contenidos/7957-22_Universidad_de_Nari%C3%B1o_Academia_de_Historia.jpg");
    background-size: cover; /* Ajusta el tamaño de la imagen al contenedor */
    background-repeat: no-repeat; /* Evita la repetición de la imagen de fondo */
    background-position: center center; /* Centra la imagen en el fondo */
  }

  .card {
    background-color: rgba(255, 255, 255, 0.6);
  }
</style>

<script>
  import Modal from "../../Forms/Modal.svelte";

  let fecha = "";
  let hora = "";

  // Función para manejar la confirmación de la selección
  function confirmarFechaHora() {
    // Aquí podrías agregar la lógica para procesar la fecha y hora seleccionadas
    console.log(`Fecha seleccionada: ${fecha}`);
    console.log(`Hora seleccionada: ${hora}`);
  }

  let mostrarTextArea = false;

  let modalVisible = false;
  let showModal = false;
  function toggleModal() {
    //showModal = !showModal;
    modalVisible = true;
  }

  function handleModalClose(event) {
    console.log("Modal cerrado, ¿se guardaron los datos?:", event.detail.saved);
    modalVisible = false;
  }
</script>

<body class="body">
  <main>
    <h5 class="card-title card text-center">Planeación De Actividades</h5>
    <div class="card">
      <div class="card card-body" style="align-items: center;">
        <div class="container text-center">
          <div class="row">
            <div class="col">
              <div class="card-header">Detalles de la Actividad</div>
              <br />
              <p>Ingrese el nombre de la actividad</p>
              <div class="input-group mb-3">
                <span class="input-group-text">
                  <div class="app-upload__icon">ℹ️</div>
                </span>
                <div class="form-floating">
                  <input
                    type="text"
                    class="form-control"
                    id="floatingInputGroup1"
                    placeholder="Username"
                  />
                  <label for="floatingInputGroup1"
                    >Nombre de la actividad
                  </label>
                </div>
              </div>
              <br />
              <div class="info-panel">
                <p>Seleccione la materia que será evaludada</p>
                <select class="form-select" aria-label="Default select example">
                  <option value="" selected hidden
                    >Ingeniería de Software I</option
                  >
                  <option value="1">Ingeniería de Software I</option>
                  <option value="2">Computación gráfica</option>
                </select>
                <br />
              </div>
              <div class="info-panel">
                <p>Seleccione la competencia a evaluar</p>
                <select class="form-select" aria-label="Default select example">
                  <option value="" selected hidden>Comunicación escrita</option>
                  <option value="1">Comunicación escrita</option>
                  <option value="2">Razamiento cuantitativo</option>
                  <option value="3">Lectura critica</option>
                  <option value="4">Competencias ciudadanas</option>
                  <option value="5">Inglés</option>
                </select>
                <br />
              </div>
            </div>
            <div class="col" style="align-items: center;">
              <div class="card-header">Ajustar Fecha y Hora</div>
              <br />
              <div class="form-check form-switch">
                <div class="container">
                  <div class="row">
                    <p>Hora de inicio</p>
                    <div class="col">
                      <input
                        type="date"
                        bind:value={fecha}
                        class="form-control"
                      />
                    </div>
                    <div class="col">
                      <input
                        type="time"
                        bind:value={hora}
                        class="form-control"
                      />
                      <br />
                    </div>
                    <p>Hora de finalización</p>
                    <div class="col">
                      <input
                        type="date"
                        bind:value={fecha}
                        class="form-control"
                      />
                    </div>
                    <div class="col">
                      <input
                        type="time"
                        bind:value={hora}
                        class="form-control"
                      />
                    </div>
                  </div>
                </div>
                <br />
                <div class="app-checkbox">
                  <label class="app-checkbox__label">
                    <input type="checkbox" class="app-checkbox__input" />
                    ¿Enviar correo de notificación a los interesados?
                  </label>
                </div>
                <br />
                <div class="app-checkbox">
                  <label class="app-checkbox__label">
                    <input type="checkbox" class="app-checkbox__input" />
                    ¿Permitir a los estudiantes subir archivos?
                  </label>
                </div>

                <div class="tipo-actividad">
                  <select
                    class="form-select"
                    aria-label="Default select example"
                  >
                    <option value="" selected hidden>Tipo Actividad</option>
                    <option value="1">Parcial</option>
                    <option value="2">Taller</option>
                    <option value="3">Laboratorio</option>
                  </select>

                  <button
                    class="btn btn-warning"
                    on:click={() => {
                      mostrarTextArea = true;
                    }}>Otro</button
                  >
                </div>

                {#if mostrarTextArea}
                  <div class="input-group mb-3">
                    <span class="input-group-text">
                      <div class="app-upload__icon">ℹ️</div>
                    </span>
                    <div class="form-floating">
                      <input
                        type="text"
                        class="form-control"
                        id="floatingInputGroup1"
                        placeholder="Username"
                      />
                      <label for="floatingInputGroup1"
                        >Tipo de activididad
                      </label>
                    </div>
                  </div>
                {/if}

                <div>
                  <button
                    class="btn btn-outline-success app-button--add-participant"
                    on:click={toggleModal}
                  >
                    <span class="app-button__icon">+</span> Adicionar participantes
                  </button>
                </div>
              </div>
              <br />
            </div>

            <div class="col">
              <div class="card-header">Subir archivo</div>
              <br />
              <div class="mb-3">
                <label for="formFile" class="form-label"
                  >Por favor suba el archivo de la prueba</label
                >
                <div class="app-upload__container">
                  <input
                    type="file"
                    id="fileInput"
                    multiple
                    class="app-upload__input"
                  />
                  <label for="fileInput" class="app-upload__label">
                    <div class="app-upload__icon">🌥️</div>
                    Choose files to Upload
                  </label>
                  <div class="app-upload__drag-text">
                    or drag and drop them here
                  </div>
                </div>
              </div>
              <br />
              <p>Su archivo es</p>
              <img
                src="https://cdn-icons-png.flaticon.com/512/80/80942.png"
                style="height: 150px;"
                class="img-fluid"
                alt="img-pdf-file"
              />
            </div>
            <div class="col">
              <div class="card-header">Ubicación de la prueba</div>
              <br />
              <div class="info-panel">
                <p class="fw-bolder">Universidad de Nariño - Sede Torobajo</p>
                <p>Seleccione el lugar de destino en la sede</p>
                <br />
                <select class="form-select" aria-label="Default select example">
                  <option value="" selected hidden>Bloque Uno</option>
                  <option value="1">Bloque Dos</option>
                  <option value="2">Bloque Tres</option>
                  <option value="3">Bloque Seis (Ingeniería)</option>
                  <option value="4">Bloque Laboratorios de Docencia</option>
                  <option value="5">Bloque Tecnologíco</option>
                  <option value="6">CESUN</option>
                </select>
                <br />
                <div class="input-group mb-3">
                  <span class="input-group-text">
                    <div class="app-upload__icon">ℹ️</div>
                  </span>
                  <div class="form-floating">
                    <input
                      type="text"
                      class="form-control"
                      id="floatingInputGroup1"
                      placeholder="Username"
                    />
                    <label for="floatingInputGroup1"
                      >Detalles de la ubicación
                    </label>
                  </div>
                </div>
              </div>
            </div>
            <button class="btn btn-success"> Guardar Cambios </button>
          </div>
        </div>
      </div>
    </div>
  </main>
</body>

{#if modalVisible}
  <Modal on:close={handleModalClose} />
{/if}

<style>
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

  .tipo-actividad {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
    gap: 20px;
    margin-bottom: 20px;
    margin-top: 20px;
  }
</style>

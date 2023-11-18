<script>
  import L from "leaflet";
  import "leaflet/dist/leaflet.css";
  import { onMount } from "svelte";

  //importamos el modalñ
  import ModalParticipantes from "./components/Forms/ModalParticipantes.svelte";
  import Modal from "./components/Forms/Modal.svelte";
  import Modal1 from "./components/Forms/Modal1.svelte";

  let fecha = "";
  let hora = "";

  // Función para manejar la confirmación de la selección
  function confirmarFechaHora() {
    // Aquí podrías agregar la lógica para procesar la fecha y hora seleccionadas
    console.log(`Fecha seleccionada: ${fecha}`);
    console.log(`Hora seleccionada: ${hora}`);
  }

  let enviarNotificacion = false;
  let participantes = [];

  function agregarParticipante() {
    // Aquí podrías abrir un modal o un formulario para agregar un nuevo participante
    // Por ejemplo:
    participantes.push({ nombre: "Nuevo Participante", email: "" });
  }

  // Función para manejar la carga de archivos
  function handleFiles(event) {
    const files = event.target.files;
    // Procesa los archivos aquí
  }

  // Función para manejar el arrastre y soltado de archivos
  function handleDrop(event) {
    event.preventDefault();
    const files = event.dataTransfer.files;
    // Procesa los archivos aquí
  }

  function preventDefault(event) {
    event.preventDefault();
  }

  let map;
  let marker;

  // Las coordenadas de la ubicación que quieres mostrar
  const location = { lat: 1.21458, lng: -77.27812 };

  // Se ejecuta en el montaje del componente
  onMount(() => {
    map = L.map("map").setView([location.lat, location.lng], 13);
    L.tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png", {
      maxZoom: 19,
      attribution: "© OpenStreetMap contributors",
    }).addTo(map);

    map.on("click", function (e) {
      let coord = e.latlng;
      let lat = coord.lat;
      let lng = coord.lng;
      if (marker) marker.remove();
      marker = L.marker([lat, lng]).addTo(map);
      // Aquí puedes actualizar un store o una variable reactiva con la ubicación
    });
  });

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

<main>
  <h5 class="card-title">Planeación Simulcaro Prueba Saber Pro</h5>
	<div class="card bg-light">
		<div class="card-body">
			<div class="container text-center">
				<div class="row">
					<div class="col" style="align-items: center;">
						<div class="card-header">Ajustar Fecha y Hora</div>
						<br />
						<div class="form-check form-switch">
							<div class="container">
								<div class="row">
									<div class="col">
										<input
											type="date"
											bind:value={fecha}
											class="form-control"
										/>
									</div>
									<div class="col">
										<input type="time" bind:value={hora} class="form-control" />
									</div>
									<div class="col">
										<br />
										<button
											class="btn btn-success"
											on:click={confirmarFechaHora}>Confirmar</button
										>
										<br />
									</div>
								</div>
							</div>
							<br />
							<div class="app-checkbox">
								<label class="app-checkbox__label">
									<input
										type="checkbox"
										class="app-checkbox__input"
										bind:checked={enviarNotificacion}
									/>
									¿Enviar correo de notificación a los interesados?
								</label>
							</div>
							<br />
							<div class="app-participants" style="align-items: center;">
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
						<br>
						<div class="mb-3">
							<label for="formFile" class="form-label"
								>Por favor suba el archivo de la prueba</label
							>
							<div
								class="app-upload__container"
								on:drop={handleDrop}
								on:dragover={preventDefault}
								on:dragenter={preventDefault}
							>
								<input
									type="file"
									id="fileInput"
									multiple
									on:change={handleFiles}
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
          <button type="button" class="btn btn-success">Enviar</button>
					<div class="col">
						<div class="card-header">Ubicación de la prueba</div>
						<br>
						<div id="map" style="height: 350px;" />
						<div class="info-panel">
							<p>
								Preview de la locación señalada disponible para ser cambiada
							</p>
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
				</div>
			</div>
		</div>
	</div>

  <!--creamos un if para comprobar si se muestran cosas-->
  {#if modalVisible}
	<Modal on:close={handleModalClose} />
  {/if}

  {#if showModal}
    <ModalParticipantes on:close={handleModalClose} />
  {/if}

  
</main>

<style>
  /* Prefijo de espacio de nombres 'app-' */
  .app-checkbox__label {
    display: flex;
    align-items: center;
    margin-bottom: 1rem; /* espacio entre elementos */
  }

  .app-checkbox__input {
    margin-right: 0.5rem;
  }

  .app-button__icon {
    display: inline-block;
    margin-right: 0.5rem;
  }

  .app-button--add-participant {
    align-items: center;
    gap: 0.5rem;
  }

  /*Estilos del De subir archivos*/

  .app-upload__container {
    border: 2px dashed #000;
    border-radius: 10px;
    padding: 20px;
    text-align: center;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    margin: auto;
    width: fit-content;
  }

  .app-upload__input {
    width: 0.1px;
    height: 0.1px;
    opacity: 0;
    overflow: hidden;
    position: absolute;
    z-index: -1;
  }

  .app-upload__label {
    font-size: 1rem;
    background-color: #fff;
    padding: 10px 20px;
    cursor: pointer;
    display: inline-flex;
    align-items: center;
    gap: 10px;
    border: 1px solid #ccc;
    border-radius: 5px;
    margin-top: 10px; /* Añadir espacio arriba del botón */
  }

  .app-upload__icon {
    font-size: 24px;
  }

  .app-upload__drag-text {
    color: #aaa;
    font-size: 0.8rem;
    margin-top: 10px;
  }

  /*Estilos del mapa*/
  #map {
    height: 400px;
  }
  .info-panel {
    margin-top: 10px;
  }
</style>

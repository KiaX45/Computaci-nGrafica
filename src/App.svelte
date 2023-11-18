<script>
  import L from 'leaflet';
  import 'leaflet/dist/leaflet.css';
  import { onMount } from 'svelte';	


  let fecha = '';
  let hora = '';

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
    participantes.push({ nombre: 'Nuevo Participante', email: '' });
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
    map = L.map('map').setView([location.lat, location.lng], 13);
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
      maxZoom: 19,
      attribution: '© OpenStreetMap contributors'
    }).addTo(map);

    map.on('click', function(e) {
      let coord = e.latlng;
      let lat = coord.lat;
      let lng = coord.lng;
      if (marker) marker.remove();
      marker = L.marker([lat, lng]).addTo(map);
      // Aquí puedes actualizar un store o una variable reactiva con la ubicación
    });
  });


</script>

<main>
	<div class="card bg-light">
		<div class="card-body">
			<h5 class="card-title">Planeación Simulcaro Prueba Saber Pro</h5>
			<div class="container text-center">
				<div class="row">
					<div class="col">
						<div class="form-check form-switch">
							<input
								class="form-check-input"
								type="checkbox"
								id="flexSwitchCheckDefault"
							/>
							<label class="form-check-label" for="flexSwitchCheckDefault"
								>¿Enviar inivtaciones por correo?</label
							>
						</div>
						<br />
					</div>
					<div class="col">
						<div class="mb-3">
							<label for="formFile" class="form-label"
								>Por favor suba el archivo de la prueba</label
							>
							<input class="form-control" type="file" id="formFile" />
						</div>
					</div>
					<div class="col">Column</div>
				</div>
			</div>
		</div>
	</div>
</main>



<div class="container">
  <div class="row">
    <div class="col">
      <input type="date" bind:value={fecha} class="form-control" />
    </div>
    <div class="col">
      <input type="time" bind:value={hora} class="form-control" />
    </div>
    <div class="col">
      <button class="btn btn-primary" on:click={confirmarFechaHora}>Confirmar</button>
    </div>
  </div>
</div>

<div class="container">
  <div class="app-checkbox">
    <label class="app-checkbox__label">
      <input type="checkbox" class="app-checkbox__input" bind:checked={enviarNotificacion}>
      ¿Enviar correo de notificación a los interesados?
    </label>
  </div>
  <div class="app-participants">
    <button class="btn btn-outline-primary app-button--add-participant" on:click={agregarParticipante}>
      <span class="app-button__icon">+</span> Adicionar participantes
    </button>
  </div>
</div>

<div class="app-upload__container"
     on:drop={handleDrop}
     on:dragover={preventDefault}
     on:dragenter={preventDefault}>
  <input type="file" id="fileInput" multiple on:change={handleFiles} class="app-upload__input">
  <label for="fileInput" class="app-upload__label">
    <div class="app-upload__icon">🌥️</div>
    Choose files to Upload
  </label>
  <div class="app-upload__drag-text">or drag and drop them here</div>
</div>


<div id="map" style="height: 400px;"></div>
<div class="info-panel">
  <p>Preview de la locación señalada disponible para ser cambiada</p>
</div>


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
  display: flex;
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

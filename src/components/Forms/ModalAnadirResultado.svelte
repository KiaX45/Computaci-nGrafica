<script>
  // Importamos createEventDispatcher para poder crear y despachar eventos personalizados
  import { createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher();

  // Este es el estado local del formulario, que recogerá los valores de los inputs

  export let formData = {
    Id: "",
    Nombre: "",
    Calificación: 0.0,
    Examen: "",
    CE: 0,
    RC: 0,
    LC: 0,
    CC: 0,
    I: 0,
  };

  // Esta función se llama cuando el formulario se envía
  function handleSubmit() {
      //Comprobamos que los datos sean correctos
    //primero que todos los datos se llenaron correctamente
    if (
      formData.Nombre == "" ||
      formData.Examen == "" ||
      formData.Calificación == 0
    ) {
      alert("Por favor llene todos los datos");
      return;
    }

    //comprobamos que la calificación este en el rango correcto
    if (formData.Calificación < 0 || formData.Calificación > 300) {
      alert("La calificación debe estar entre 0 y 300");
      return;
    }

    //comprobamos que las competencias esten en el rango correcto en un rango de 0 a 300
    if (
      formData.CE < 0 ||
      formData.CE > 300 ||
      formData.RC < 0 ||
      formData.RC > 300 ||
      formData.LC < 0 ||
      formData.LC > 300 ||
      formData.CC < 0 ||
      formData.CC > 300 ||
      formData.I < 0 ||
      formData.I > 300
    ) {
      alert("Las competencias deben estar entre 0 y 300");
      return;
    }
    console.log("Formulario enviado");
    alert(
      "Se agregara el registro con el ID ¨" +
        formData.Id +
        "¨ el Nombre ¨" +
        formData.Nombre +
        "¨ la calificacion ¨" +
        formData.Calificación +
        "¨ y la prueba ¨" +
        formData.Examen
    );
    //comprobamos si los datos son correctos
    
      // Despachamos un evento personalizado llamado 'close' con los datos del formulario
      dispatch("close", formData);
    
  }

  function handleCancel() {
   
      console.log("Formulario cancelado");
      // Despachamos un evento personalizado llamado 'close' con los datos del formulario
      dispatch("close", null);
  }

  // Esta función se llama para cerrar el modal sin enviar datos
  function handleClose() {
    console.log("Modal cerrado");
    // Despachamos el evento 'close' con valor null para indicar que se cerró sin acción
    dispatch("close", null);
  }
</script>

<!-- Estructura del modal -->
<div class="modal show" tabindex="-1" style="display: block;">
  <div class="modal-dialog">
    <div class="modal-content" style="width: 600px;">
      <div class="modal-header">
        <h5 class="modal-title">Ingrese los nuevos datos</h5>
        <!-- Botón para cerrar el modal -->
        <button
          type="button"
          class="btn-close"
          aria-label="Close"
          on:click={handleClose}
        />
      </div>
      <div class="modal-body">
        <!-- El formulario recoge los datos y previene el comportamiento por defecto del submit -->
        <form>
          <!-- Campos visuales -->
          <div class="container text-center">
            <!--Creamos un formulario para los nuevos resultasdos-->
            <div class="row">
              <div class="col-md-3">
                <label for="nombre">Id</label>
                <input
                  type="number"
                  class="form-control"
                  id="nombre"
                  placeholder="ID"
                  min="0"
                  bind:value={formData.Id}
                />
              </div>
              <div class="col-md-6">
                <label for="nombre">Nombre</label>
                <input
                  type="text"
                  class="form-control"
                  id="nombre"
                  placeholder="Nombre"
                  bind:value={formData.Nombre}
                />
              </div>
              <div class="col-md-3">
                <label for="examen">Examen</label>
                <input
                  type="text"
                  class="form-control"
                  id="examen"
                  placeholder="Examen"
                  bind:value={formData.Examen}
                />
              </div>
            </div>
            <div style="height: 20px;"></div>
            <!--Creamos otra fila para poner las competencias de las calificaciones-->
            <div class="row" >
              <div class="col">
                <label for="nombre">CE</label>
                <input
                  type="number"
                  class="form-control"
                  id="nombre"
                  placeholder="CE"
                  min="0"
                  max="100"
                  bind:value={formData.CE}
                />
              </div>
              <div class="col">
                <label for="nombre">RC</label>
                <input
                  type="number"
                  class="form-control"
                  id="nombre"
                  placeholder="RC"
                  min="0"
                  max="100"
                  bind:value={formData.RC}
                />
              </div>
              <div class="col">
                <label for="calificacion">LC</label>
                <input
                  type="number"
                  class="form-control"
                  id="calificacion"
                  placeholder="LC"
                  min="0"
                  max="100"
                  bind:value={formData.LC}
                />
              </div>
              <div class="col">
                <label for="examen">CC</label>
                <input
                  type="number"
                  class="form-control"
                  id="examen"
                  placeholder="CC"
                  min="0"
                  max="100"
                  bind:value={formData.CC}
                />
              </div>
              <div class="col">
                <label for="examen">I</label>
                <input
                  type="number"
                  class="form-control"
                  id="examen"
                  placeholder="I"
                  min="0"
                  max="100"
                  bind:value={formData.I}
                />
              </div>
            </div>
             <div style="height: 20px;"></div>

            <div class="row">
              <div class="col-md-4 mx-auto">
                <label for="calificacion">Calificación</label>
                <input
                  type="number"
                  class="form-control"
                  id="calificacion"
                  placeholder="Calificación"
                  min="0"
                  max="300"
                  bind:value={formData.Calificación}
                />
              </div>

            </div>

            <div style="height: 20px;"></div>
            <!-- Botón para enviar el formulario -->
            <button
              type="submit"
              class="btn btn-success"
              on:click|preventDefault={handleSubmit}>Guardar</button
            >
            <button type="submit" class="btn btn-danger" on:click|preventDefault={handleCancel}
              >Cancelar</button
            >
          </div>
        </form>
      </div>
    </div>
  </div>
</div>
<!-- Fondo oscuro detrás del modal -->
<div class="modal-backdrop fade show" />

<style>
  /* Estilos adicionales para asegurarse de que el modal se muestre correctamente */
  .modal.show {
    display: block;
  }
  .modal-backdrop {
    position: fixed;
    top: 0;
    left: 0;
    z-index: 1040;
    width: 100vw;
    height: 100vh;
    background-color: #000;
    opacity: 0.5;
  }
</style>

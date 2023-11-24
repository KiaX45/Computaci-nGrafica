<!-- ModalForm.svelte -->
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
  };

  // Esta función se llama cuando el formulario se envía
  function handleSubmit() {
    console.log("Formulario enviado");
    //comprobamos si los datos son correctos
    if(formData.Calificación<0 || formData.Calificación>5 || formData.Id<0){
      alert("verifique correctamente los campos");
      return;
    }  
    if (formData.Nombre == "" || formData.Calificación == 0.0 || formData.Examen == "") {
      alert("Por favor ingrese todos los datos");
    } else {
      // Despachamos un evento personalizado llamado 'close' con los datos del formulario
      dispatch("close", formData);
    }
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
    <div class="modal-content">
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
              <div class="col">
                <label for="nombre">Id</label>
                <input
                  type="number"
                  class="form-control"
                  id="nombre"
                  placeholder="ID"
                  bind:value={formData.Id}
                />
              </div>
              <div class="col">
                <label for="nombre">Nombre</label>
                <input
                  type="text"
                  class="form-control"
                  id="nombre"
                  placeholder="Nombre"
                  bind:value={formData.Nombre}
                />
              </div>
              <div class="col">
                <label for="calificacion">Calificación</label>
                <input
                  type="number"
                  class="form-control"
                  id="calificacion"
                  placeholder="Calificación"
                  bind:value={formData.Calificación}
                />
              </div>
              <div class="col">
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
           
          </div>
          <div style="height: 20px;"></div>
          <!-- Botón para enviar el formulario -->
          <button type="submit" class="btn btn-success" on:click|preventDefault={handleSubmit}>Guardar</button>
          <button type="submit" class="btn btn-danger" on:click={handleCancel}>Cancelar</button>
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

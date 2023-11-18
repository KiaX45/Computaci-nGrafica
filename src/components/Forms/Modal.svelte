<!-- ModalForm.svelte -->
<script>
  // Importamos createEventDispatcher para poder crear y despachar eventos personalizados
  import { createEventDispatcher } from 'svelte';
  const dispatch = createEventDispatcher();

  // Este es el estado local del formulario, que recogerá los valores de los inputs
  let formData = {
    ncs: 5,
    es: 0,
    xi: 4.2,
    xs: 4.3
  };
 

  // Esta función se llama cuando el formulario se envía
  function handleSubmit() {
    // Despachamos un evento personalizado llamado 'submit' con los datos del formulario
    dispatch('close', formData);
    // Despachamos otro evento para indicar al componente padre que cierre el modal
    dispatch('closeModal');
  }

  // Esta función se llama para cerrar el modal sin enviar datos
  function handleClose() {
    dispatch('closeModal');
  }
</script>

<!-- Estructura del modal -->
<div class="modal show" tabindex="-1" style="display: block;">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title">Datos del Método Numérico</h5>
        <!-- Botón para cerrar el modal -->
        <button type="button" class="btn-close" aria-label="Close" on:click={handleClose}></button>
      </div>
      <div class="modal-body">
        <!-- El formulario recoge los datos y previene el comportamiento por defecto del submit -->
        <form on:submit|preventDefault={handleSubmit}>
          <!-- Campos del formulario -->
          <div class="mb-3">
            <label for="ncs" class="form-label">NCS</label>
            <input type="number" step="any" class="form-control" id="ncs" bind:value={formData.ncs}>
          </div>
          <div class="mb-3">
            <label for="xi" class="form-label">XI</label>
            <input type="number" step="any" class="form-control" id="xi" bind:value={formData.xi}>
          </div>
          <div class="mb-3">
            <label for="xs" class="form-label">XS</label>
            <input type="number" step="any" class="form-control" id="xs" bind:value={formData.xs}>
          </div>
          <!-- Botón para enviar el formulario -->
          <button type="submit" class="btn btn-success">Enviar</button>
        </form>
      </div>
    </div>
  </div>
</div>
<!-- Fondo oscuro detrás del modal -->
<div class="modal-backdrop fade show"></div>

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

  .btn {
    background-color: #519259;
  }
</style>

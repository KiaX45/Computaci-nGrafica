<!-- ModalForm.svelte -->
<script>
  // Importamos createEventDispatcher para poder crear y despachar eventos personalizados
  import { createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher();

  // Este es el estado local del formulario, que recogerá los valores de los inputs
  let formData = {
    ncs: 5,
    es: 0,
    xi: 4.2,
    xs: 4.3,
  };

  // Esta función se llama cuando el formulario se envía
  function handleSubmit() {
    // Despachamos un evento personalizado llamado 'submit' con los datos del formulario
    dispatch("close", formData);
    // Despachamos otro evento para indicar al componente padre que cierre el modal
    dispatch("closeModal");
  }

  // Esta función se llama para cerrar el modal sin enviar datos
  function handleClose() {
    dispatch("close", formData);
  }
</script>

<!-- Estructura del modal -->
<div class="modal show" tabindex="-1" style="display: block;">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title">Seleccione los estudiantes a participar</h5>
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
        <form on:submit|preventDefault={handleSubmit}>
          <!-- Campos visuales -->
          <div class="container text-center">
            <div class="row">
              <div class="col">
                <select class="form-select" aria-label="Default select example">
                  <option value="" selected hidden>Sexto Semestre</option>
                  <option value="1">Primer Semestre</option>
                  <option value="2">Segundo Semestre</option>
                  <option value="3">Tercer Semestre</option>
                  <option value="4">Cuarto Semestre</option>
                  <option value="5">Quinto Semestre</option>
                  <option value="6">Sexto Semestre</option>
                  <option value="7">Septimo Semestre</option>
                  <option value="8">Octavo Semestre</option>
                  <option value="9">Noveno Semestre</option>
                  <option value="10">Decimo Semestre</option>
                </select>
                <br />
              </div>
              <div class="col">
                <div class="row">
                  <label
                    for="colFormLabelSm"
                    class="col-sm-2 col-form-label col-form-label-sm"
                    >Buscar</label
                  >
                  <div class="col-sm-10">
                    <input
                      type="email"
                      class="form-control form-control-sm"
                      id="colFormLabelSm"
                      placeholder="Ingrese un Nombre"
                    />
                  </div>
                </div>
              </div>
            </div>
            <table class="table table-success table-striped table-bordered">
              <thead>
                <tr>
                  <th>No.</th>
                  <th>Semestre</th>
                  <th>Nombres</th>
                  <th>Apellidos</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td>1</td>
                  <td>6</td>
                  <td>Yesid</td>
                  <td>Bolaños</td>
                </tr>
                <tr>
                  <td>3</td>
                  <td>6</td>
                  <td>Jimena</td>
                  <td>Cabrera</td>
                </tr>
                <tr>
                  <td>4</td>
                  <td>6</td>
                  <td>Gabriel</td>
                  <td>Peña</td>
                </tr>
                <tr>
                  <td>5</td>
                  <td>6</td>
                  <td>Cristian</td>
                  <td>Burgos</td>
                </tr>
                <tr>
                  <td>6</td>
                  <td>6</td>
                  <td>Ysela</td>
                  <td>Creyo</td>
                </tr>
              </tbody>
            </table>
          </div>
          <!-- Botón para enviar el formulario -->
          <button type="submit" class="btn btn-success">Enviar</button>
          <button type="submit" class="btn btn-danger">Cancelar</button>
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

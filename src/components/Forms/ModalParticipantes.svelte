<script>
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();
  let searchTerm = "";

  function guardar() {
    dispatch("close", { saved: true });
  }

  function cancelar() {
    dispatch("close", { saved: false });
  }
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<div class="modal-overlay" on:click={() => dispatch("close", { saved: false })}>
  <div class="modal" on:click|stopPropagation>
    <div class="modal-content">
      <div class="modal-header">
        <select class="dropdown">
          <option>Seleccione el semestre</option>
          <!-- Opciones de semestre -->
        </select>
        <input
          type="search"
          class="search-input"
          placeholder="Buscar Participante"
          bind:value={searchTerm}
        />
      </div>
      <div class="modal-body">
        <div class="list-container">
          <h3>Semestres</h3>
          <div class="list-scroll">
            <!-- Lista de semestres -->
          </div>
        </div>
        <div class="list-container">
          <h3>Participantes</h3>
          <div class="participants-grid">
            <!-- Grid de participantes -->
          </div>
        </div>
      </div>
      <div class="modal-footer">
        <button class="btn-save" on:click={guardar}>Guardar</button>
        <button class="btn-cancel" on:click={cancelar}>Cancelar</button>
      </div>
    </div>
  </div>
</div>

<style>
  .modal-overlay {
    display: flex;
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.6);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 999; /* Alto z-index para asegurarse de que se muestre encima de todo */
  }

  .modal {
    display: block;
    background-color: white;
    border-radius: 4px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
    position: relative; /* Se añade posición relativa para el z-index del hijo */
    z-index: 1000; /* Asegurarse de que el modal está encima del overlay */
  }

  .modal-content {
    padding: 20px;
    max-width: 800px;
    min-width: 300px; /* Añadir un ancho mínimo para que sea visible en pantallas pequeñas */
    max-height: 90vh; /* Ajustar la altura máxima para que no sobrepase la pantalla */
    overflow-y: auto; /* Agregar scroll si el contenido es muy alto */
    position: relative; /* Posición relativa para el contenido */
  }

  .modal-header {
    display: flex;
    justify-content: space-between;
    margin-bottom: 20px;
  }
  .dropdown,
  .search-input {
    padding: 10px;
    margin-right: 10px;
    border: 1px solid #ccc;
    border-radius: 4px;
  }
  .modal-body {
    display: flex;
    justify-content: space-between;
  }
  .list-container {
    width: calc(50% - 10px);
  }
  .list-scroll {
    border: 1px solid #ccc;
    padding: 10px;
    height: 300px; /* O la altura que prefieras */
    overflow-y: auto;
  }
  .participants-grid {
    border: 1px solid #ccc;
    padding: 10px;
    height: 300px; /* O la altura que prefieras */
    overflow-y: auto;
    /* Estilos para la grilla de participantes */
  }
  .modal-footer {
    text-align: right;
    padding-top: 20px;
  }
  .btn-save,
  .btn-cancel {
    padding: 10px 20px;
    margin-left: 10px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }
  .btn-save {
    background-color: #4caf50; /* Verde */
    color: white;
  }
  .btn-cancel {
    background-color: #f44336; /* Rojo */
    color: white;
  }
  /* ... otros estilos ... */
</style>

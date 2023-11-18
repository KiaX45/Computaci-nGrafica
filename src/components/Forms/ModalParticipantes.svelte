<script>
  import { createEventDispatcher } from 'svelte';

  const dispatch = createEventDispatcher();
  let searchTerm = '';

  function guardar() {
    // Lógica de guardado
    dispatch('close', { saved: true });
  }

  function cancelar() {
    // Lógica de cancelación
    dispatch('close', { saved: false });
  }
</script>

<div class="modal-overlay" on:click={() => dispatch('close', { saved: false })}>
  <div class="modal" on:click|stopPropagation>
    <div class="modal-content">
      <div class="modal-header">
        <select class="dropdown">
          <option>Seleccione el semestre</option>
          <!-- Opciones de semestre -->
        </select>
        <input type="search" class="search-input" placeholder="Buscar Participante" bind:value={searchTerm} />
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
    /* ... estilos del overlay ... */
  }
  .modal {
    /* ... estilos del modal ... */
  }
  .modal-content {
    background-color: white;
    padding: 20px;
    border-radius: 8px;
    width: 800px; /* O el ancho que prefieras */
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
  }
  .modal-header {
    display: flex;
    justify-content: space-between;
    margin-bottom: 20px;
  }
  .dropdown, .search-input {
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
  .btn-save, .btn-cancel {
    padding: 10px 20px;
    margin-left: 10px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }
  .btn-save {
    background-color: #4CAF50; /* Verde */
    color: white;
  }
  .btn-cancel {
    background-color: #f44336; /* Rojo */
    color: white;
  }
  /* ... otros estilos ... */
</style>

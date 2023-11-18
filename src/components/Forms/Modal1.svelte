<script>
  import { createEventDispatcher } from 'svelte';

  const dispatch = createEventDispatcher();

  // Para cerrar el modal, emitimos un evento personalizado que el componente padre puede escuchar.
  function closeModal() {
    dispatch('close');
  }
</script>

<div class="modal-overlay" on:click={closeModal}>
  <div class="modal-content" on:click|stopPropagation>
    <div class="modal-header">
      <button on:click={closeModal} class="close-button">&times;</button>
    </div>
    <div class="modal-body">
      <p>Contenido del modal...</p>
    </div>
    <div class="modal-footer">
      <button on:click={closeModal}>Cerrar</button>
    </div>
  </div>
</div>

<style>
  .modal-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 1000;
  }
  .modal-content {
    background-color: white;
    padding: 20px;
    border-radius: 4px;
    box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
    position: relative;
    z-index: 1001;
  }
  .modal-header {
    display: flex;
    justify-content: flex-end;
  }
  .close-button {
    background: none;
    border: none;
    font-size: 1.5em;
    cursor: pointer;
  }
  .modal-body {
    padding: 10px 0;
  }
  .modal-footer {
    text-align: right;
    padding-top: 10px;
  }
</style>

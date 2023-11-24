<script>
  //importamos la librería para leer archivos excel
  import * as XLSX from "xlsx";

  //importamos el componente de la lista de resultados
  import ListadeResultadosCalificacion from "../smallcomponents/ListadeResultadosCalificacion.svelte";
  //importamos el modal de añadir resultados
  import ModalAnadirResultado from "../../Forms/ModalAnadirResultado.svelte";

  const handleFileInput = (e) => {
    console.log("Se ha seleccionado un archivo");
    //creamos un objeto para guardar el archivo enviado
    const file = e.target.files[0]; //se poner files porque es un array de archivos

    //validamos que el archivo no sea nulo
    if (!file) {
      console.error("No file selected");
      // Mostrar un mensaje de error al usuario
      return;
    }

    console.log(file.name);
    //validamos que el archivo sea de tipo csv o excel
    if (!file.name.endsWith(".csv") && !file.name.endsWith(".xlsx")) {
      console.error("Solo se permiten archivos de tipo csv o excel");
      // Mostrar un mensaje de error al usuario
      return;
    }

    //validamos que el archivo no sea mayor a 1MB
    if (file.size > 1024 * 1024) {
      console.error("El archivo no puede ser mayor a 1MB");
      // Mostrar un mensaje de error al usuario
      return;
    }

    //llaamamos la función para leer el archivo
    read(file);
  };

  //Creamos una variable para mostrar el componente de la lista de resultados
  let listaResultadosVisible = false;

  //Creamos una variable para almacenar los datos del archivo
  let result;

  //Función para leer el archivo
  const read = (file) => {
    const reader = new FileReader();
    reader.onload = (e) => {
      const data = new Uint8Array(e.target.result);
      const workbook = XLSX.read(data, { type: "array" });

      // Suponiendo que tu Excel tiene una sola hoja y quieres leerla
      const firstSheetName = workbook.SheetNames[0];
      const worksheet = workbook.Sheets[firstSheetName];

      // Convierte la hoja en un array de objetos
      const json = XLSX.utils.sheet_to_json(worksheet);
      console.log(json); // Resultado final
      result = json;
      //mostramos el componente de la lista de resultados
      listaResultadosVisible = true;
    };
    reader.readAsArrayBuffer(file);
  };

  //Vamos a crear un función para enviar un unico dato al componente lista en caso de que el usuario no haya subido un archivo
  //Para eso vamos a utilizar el modal ya creado para añadir un resultado

  //Creamos una variable para mostrar el modal
  let modalAnadirResultadoVisible = false;

  //creamos una función para mostrar el modal
  function mostrarModalAnadirResultado() {
    modalAnadirResultadoVisible = true;
  }

  //creamos una variable para almacenar los datos del archivo
  let resultadoIndividual = null;

  //creamos una función para recibir los datos de un modal cerrar el modal y mostrar la tabla de resultados enviando un solo dato
  function handleAddModalClose(event) {
    if (event.detail == null) {
      console.log("No se ha recibido ningún dato");
      //cerramos el modal
      modalAnadirResultadoVisible = false;
      return;
    }

    //recibimos los datos del modal
    resultadoIndividual = event.detail;
    console.log(result);

    //cerramos el modal
    modalAnadirResultadoVisible = false;

    //mostramos la tabla de resultados
    listaResultadosVisible = true;

    //enviamos los datos a la lista de resultados
  }

  //CReamos una funció para controlar el evento close de la lista de notas
  const handleListClose = (event) => {
    console.log("Se ha cerrado la lista de resultados");
    listaResultadosVisible = false;
  };
</script>

<main>
  <div class="card text-center">
    <h5 class="card-header">Seguimiento de las pruebas</h5>
    <div class="card-body" style="justify-items: center;">
      {#if listaResultadosVisible}
        <ListadeResultadosCalificacion
          json={result}
          result={resultadoIndividual}
          on:close={handleListClose}
        />
      {:else}
      <div class="container text-center">
        <div class="row">
          <div class="col">
            <p>Puede subir su archivo de calificaciones aquí</p>
            <input type="file" name="" id="" on:change={handleFileInput} />
          </div>
          <div class="col">
            <p>Puede agregar calificaciones desde aquí</p>
            <button class="btn btn-success" on:click={mostrarModalAnadirResultado}
              >Añadir resultados</button
            >
          </div>
        </div>
      </div>
      {/if}

      {#if modalAnadirResultadoVisible}
        <ModalAnadirResultado on:close={handleAddModalClose} />
      {/if}
    </div>
  </div>
</main>

<style>
</style>

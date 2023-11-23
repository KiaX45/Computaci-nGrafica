<script>
//importamos la librería para leer archivos excel
import * as XLSX from 'xlsx';

//importamos el componente de la lista de resultados
import ListadeResultadosCalificacion from "../smallcomponents/ListadeResultadosCalificacion.svelte";




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
            const workbook = XLSX.read(data, { type: 'array' });

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


</script>

<main>
  <h1>Suba el archivo de las calificaciones</h1>
  <input type="file" name="" id="" on:change={handleFileInput} />

  {#if listaResultadosVisible}
    <ListadeResultadosCalificacion json={result} />
  {/if}
</main>



<style>
</style>

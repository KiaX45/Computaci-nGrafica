<script>
  //importamos el componenete onMount
  import { onMount } from "svelte";
  //importamos el modal para editar datos 
import ModalEditarResultadoExamen from "../../Forms/ModalEditarResultadoExamen.svelte";
  //Esta es la lista de resultados cuando el usuario ingresa un archivo de excel con las notas de los estudiantes

  //creamos una variable que reciba los datos del archivo
  export let json;
  //Creamos una variable que almacene los resultados en individual y otra que almacene todos los resultados
  let resultado = {
    Id: "",
    Nombre: "",
    Calificación: "",
    Examen: "",
  };
  let resultados = [];

  //Creamos una variable para indicar cuando se debe mostrar la tabla
  let tablaVisible = false;

  //Utilizamos onMount para que cuando se cargue el componente se ejecute la función
  onMount(() => {
    try {
      //Llamamos la función para recuperar los datos del archivo
      comprobarDatos();
    } catch (error) {
      console.log(error);
    }
  });

  async function comprobarDatos() {
    try {
      //Llamamos la función para recuperar los datos del archivo
      await getData();
      tablaVisible = true;
    } catch (error) {
      console.log(error);
    }
  }

  //Creamos una funcion asincrona para recuperar los datos del json
  async function getData() {
    resultados = json.map((element) => ({
      Id: element.Id,
      Nombre: element.Nombre,
      Calificación: element.Calificación,
      Examen: element.Examen,
      Error: false,
    }));

    //Llamamos la función para verificar que todas las notas estan en un rango valido
    verificarNotas();
  }

  //Creamos una variable para saber cuantos errores en total existen
  let erroresTotal = 0;

  //Creamos una función para verificar que todas las notas estan en un rango valido
  function verificarNotas() {
    //Recorremos el arreglo de resultados
    resultados.forEach((element) => {
      //Verificamos que la nota este en el rango de 0 a 5
      if (element.Calificación < 0 || element.Calificación > 5) {
        //Si la nota no esta en el rango valido, entonces marcamos el error
        element.Error = true;
        //Aumentamos el contador de errores
        erroresTotal++;
      }
    });
  }

  //Creamos una variable para mostrar el modal de edición de resultados
  let modalEditarVisible = false;
  //Creamos una función para editar alguno de los resultados
  const editarResultado = (elecment) =>{
    resultado = elecment;
    modalEditarVisible = true;
    console.log(elecment);
  }
  //Creamos una función que se ejecuta cuando se cierra el modal
  function handleModalClose(event) {
  if(event.detail !== null){
    console.log(event.detail);
    console.log("Se guardaron los datos");
    // Actualizamos
    tablaVisible = false;
    let updated = false;

    resultados = resultados.map((element) => {
      if(element.Id === event.detail.Id){
        updated = true;
        return {
          ...element,
          Nombre: event.detail.Nombre,
          Calificación: event.detail.Calificación,
          Examen: event.detail.Examen,
        };
      }
      return element;
    });

    if (updated) {
      console.log(resultados);
    } else {
      console.log("ID no encontrado en los resultados.");
    }
    
    tablaVisible = true;
  } else {
    console.log("No se guardaron los datos");
  }

  modalEditarVisible = false;
}


  
</script>

<main>
  <h1>Lista de resultados</h1>
  <!--Creamos una tabla para mostrar los resultados-->
  {#if tablaVisible}
    <table>
      <!--Creamos el encabesado de la tabla-->
      <thead>
        <tr>
          <th>Id</th>
          <th>Nombre</th>
          <th>Calificación</th>
          <th>Examen</th>
          <th>Editar</th>
          <th>Remover</th>
        </tr>
      </thead>

      <tbody>
        <!--Vamos a crear una tabla en base  a los resultados que tenemos-->
        {#each resultados as element}
          <tr>
            <td>{element.Id}</td>
            <td>{element.Nombre}</td>
            <!--Cambiamos el color del fondo si la nota esta en un rango no valido-->
            {#if element.Error}
              <td style="background-color: red;">{element.Calificación}</td>
            {:else}
              <td>{element.Calificación}</td>
            {/if}
            <td>{element.Examen}</td>
            <!--Creamos un boton editar con bootstrap para que cuando se le de click edite el elemento-->
            <td>
              <button class="btn btn-warning" on:click={editarResultado(element)}>Editar</button>
            </td>

            <td>
              <button class="btn btn-danger" on:click={editarResultado(element)}>Remover</button>
            </td>

          </tr>
        {/each}
      </tbody>
    </table>
  {/if}

  <!--Creamos el modal para editar los resultados y resivimos los datos que nos envia para mandarlos por parametro-->
  {#if modalEditarVisible}
    <ModalEditarResultadoExamen on:close={handleModalClose} formData={resultado}/>
  {/if}  

</main>



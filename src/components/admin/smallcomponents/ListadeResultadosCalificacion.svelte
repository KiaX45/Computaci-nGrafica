<script>
  //importamos el componenete onMount
  import { onMount } from "svelte";
  //importamos el modal para editar datos
  import ModalEditarResultadoExamen from "../../Forms/ModalEditarResultadoExamen.svelte";
  //importamos el modal para añadir datos
  import ModalAnadirResultado from "../../Forms/ModalAnadirResultado.svelte";
  //Esta es la lista de resultados cuando el usuario ingresa un archivo de excel con las notas de los estudiantes
  import { createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher();
  //creamos una variable que reciba los datos del archivo
  export let json;
  //Creamos una variable para que reciba un solo dato en caso de que no se haya subido un archivo
  export let result;
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
    //comprobamos si solo se subio un dato
    if (result != null) {
      //si solo se subio un dato entonces lo guardamos en la variable de resultados
      resultados = resultados.concat(result);
      //Llamamos la función para verificar que todas las notas estan en un rango valido
      verificarNotas();
      return;
    }

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
  const editarResultado = (elecment) => {
    resultado = elecment;
    modalEditarVisible = true;
    console.log(elecment);
  };
  //Creamos una función que se ejecuta cuando se cierra el modal
  function handleModalClose(event) {
    if (event.detail !== null) {
      console.log(event.detail);
      console.log("Se guardaron los datos");
      // Actualizamos
      tablaVisible = false;
      let updated = false;

      resultados = resultados.map((element) => {
        if (element.Id === event.detail.Id) {
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

  //Funciones para remover un resultado
  function removerResultado(event) {
    var IsConfirm = confirm(
      "¿Esta seguro?, ¡esta accion no se puede deshacer!",
    );
    if (IsConfirm) {
      resultados = resultados.filter((element) => element.Id !== event.Id);
      console.log(event);
      alert("Se eliminara el registro");
      console.log(resultados);
    } else {
      alert("Accion cancelada");
    }
  }

  //CReamos una variable para mostrar el modal de añadir resultados
  let modalAñadirVisible = false;

  //Creamos una función para añadir un nuevo resultado
  function añadirResultado() {
    modalAñadirVisible = true;
  }

  //Creamos una función que se ejecuta cuando se cierra el modal
  const handleAddModalClose = (event) => {
    if (event.detail == null) {
      console.log("No se enviaron los datos");
    } else {
      console.log(event.detail);
      resultado = event.detail;

      //Verificamos que el id no exista
      let existe = false;
      resultados.forEach((element) => {
        if (element.Id === resultado.Id) {
          existe = true;
        }
      });
      if (existe) {
        alert("El id ya existe");
        return;
      }
      resultados = resultados.concat(resultado);
    }
    modalAñadirVisible = false;
  };

  //Función para ir al inicio de la página
  function scrollToTop() {
    window.scrollTo({ top: 0, behavior: "smooth" });
  }

  //Función para enviar los datos
  const EnviarDatos = () => {
    //comprobamos si hay errores
    if (erroresTotal > 0) {
      alert("Verifique que todas las notas esten en un rango valido");
      return;
    }
    //comprobamos si hay datos
    if (resultados.length == 0) {
      alert("No hay datos para enviar");
      return;
    }
    //enviamos los datos
    console.log(resultados);

    //Despachamos un evento personalizado llamado 'close' para indicarle al componente padre que se enviaron los datos
    dispatch("close", null);
  };
</script>

<body>
  <div class="container text-center">
    <div class="row">
      <div class="col">
        <p>¿Desea agregar mas resultados?</p>
      </div>
      <div class="col">
        <button class="btn btn-success" on:click={añadirResultado}
          >Añadir</button
        >
      </div>
    </div>
  </div>
  <br />
  <p>El contenido del archivo seleccionado es:</p>
  <br />

  <!--Creamos una tabla para mostrar los resultados-->
  {#if tablaVisible}
    <table class="table table-striped">
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
              <button
                class="btn btn-warning"
                on:click={editarResultado(element)}>Editar</button
              >
            </td>

            <td>
              <button
                class="btn btn-danger"
                on:click={removerResultado(element)}>Remover</button
              >
            </td>
          </tr>
        {/each}
      </tbody>
    </table>

    <p>Estiamdo usuario tenga en cuenta que si la tabla contiene franjas rojas no se podrán ejecutar cambios</p>
    <!--Creamos un boton para ir al inicio de la página-->
    <button class="btn btn-primary" on:click={scrollToTop}>Ir al inicio</button>
    <button class="btn btn-success" on:click={EnviarDatos}>Enviar</button>
  {/if}
</body>

<!--Creamos el modal para editar los resultados y recibimos los datos que nos envia para mandarlos por parametro-->
{#if modalEditarVisible}
  <ModalEditarResultadoExamen
    on:close={handleModalClose}
    formData={resultado}
  />
{/if}

<!--Creamos el modal para añadir los resultados-->
{#if modalAñadirVisible}
  <ModalAnadirResultado on:close={handleAddModalClose} />
{/if}

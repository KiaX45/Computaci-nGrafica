<script>
  import { onMount, onDestroy } from "svelte";
  import Chart from "chart.js/auto";
  import { createEventDispatcher } from 'svelte';




  export let codigo = "";
  let resultadoIcfes = null;
  let isLoading = false;
  let error = null;
  let myChart;
  const dispatch = createEventDispatcher();

  //Metodo para cargar los resultados del estudiante
  const cargarResultados = async () => {
    error = null;
    try {
      isLoading = true;
      const response = await fetch(
        `http://localhost:8080/api/estudiantes/${codigo}/simulacros`
      );
      if (!response.ok) {
        throw new Error("Error en la respuesta de la API");
      }
      resultadoIcfes = await response.json();
      dispatch('enviarDatos', resultadoIcfes);
    } catch (e) {
      
      error =  "No se encontraron resultados con el código del estudiante proporcionado";
        // "No se encontraron resultados con el código del estudiante proporcionado";
        //ponemos resultados por defecto para testear
        //dispatch("enviarDatos", null);
    } finally {
      isLoading = false;
    }
  };

  //Reactividad del componente para que se ejecute cada vez que cambie el codigo
  $: cargarResultados(), codigo;

  let canvasElement;

  // Reactiva: inicializa el gráfico una vez que los datos y el canvas están disponibles
  $: {
    if (resultadoIcfes && canvasElement) {
      inicializarGrafico();
    }
  }

  function inicializarGrafico() {
    if (myChart) {
      myChart.destroy(); // Destruye la instancia anterior del gráfico si existe
    }

    const ctx = canvasElement.getContext("2d");
    // Aquí procesarías 'resultadosEstudiante' para obtener los datos del gráfico
    const datosGrafico = procesarDatos(resultadoIcfes);

    myChart = new Chart(ctx, {
      type: "bar",
      data: {
        labels: [
          "Lectura",
          "Matematicas",
          "Sociales",
          "Naturales",
          "Inglés",
          "Resultado",
        ],
        datasets: [
          {
            label: "Calificación Promedio por Sección",
            data: datosGrafico,
            backgroundColor: [
              "rgb(255, 99, 132)", // Rojo
              "rgb(54, 162, 235)", // Azul
              "rgb(255, 206, 86)", // Amarillo
              "rgb(75, 192, 192)", // Verde
              "rgb(153, 102, 255)", // Púrpura
            ],
            borderColor: [
              "rgb(255, 99, 132)",
              "rgb(54, 162, 235)",
              "rgb(255, 206, 86)",
              "rgb(75, 192, 192)",
              "rgb(153, 102, 255)",
            ],
            borderWidth: 0.2,
          },
        ],
      },
      options: {
        responsive: true, // Hace que el gráfico sea responsivo
        maintainAspectRatio: false, // Desactiva la relación de aspecto para que el gráfico ocupe todo el contenedor
        scales: {
          y: {
            beginAtZero: true,
            ticks: {
              fontSize: 14, // Tamaño de la fuente para los ticks del eje Y
            },
          },
          x: {
            ticks: {
              fontSize: 14, // Tamaño de la fuente para los ticks del eje X
            },
          },
        },
        legend: {
          labels: {
            fontSize: 16, // Tamaño de la fuente para la leyenda
          },
        },
        layout: {
          padding: {
            // Añadir un poco de padding alrededor del gráfico
            top: 15,
            right: 15,
            bottom: 15,
            left: 15,
          },
        },
      },
    });
  }

  const procesarDatos = (resultadosEstudiante) => {
    const datosGrafico = [];
    resultadosEstudiante.forEach((resultado) => {
      datosGrafico.push(resultado.lectura);
      datosGrafico.push(resultado.matematicas);
      datosGrafico.push(resultado.sociales);
      datosGrafico.push(resultado.naturales);
      datosGrafico.push(resultado.ingles);
      let resultadoFinal = (resultado.lectura + resultado.matematicas + resultado.sociales + resultado.naturales + resultado.ingles) / 5;
      datosGrafico.push(resultadoFinal);
    });
    return datosGrafico;
  };

  onDestroy(() => {
    if (myChart) {
      myChart.destroy();
    }
  });
</script>

{#if isLoading}
  <p>Cargando...</p>
{:else if error}
  <div class="alert alert-danger">{error}</div>
{:else if resultadoIcfes}
  <div class="Información">
      <h1 class="titulo-con-fondo">Prueba Saber 11</h1>
    </div>
  <div class="chart-container">
    <canvas bind:this={canvasElement}></canvas>
  </div>
{/if}

<style>
  .chart-container {
    /* Ajusta estas dimensiones según las necesidades de tu diseño */
    width: 70%;
    height: 800px;
    margin: auto; /* Centra el contenedor si es más pequeño que el viewport */
  }

  canvas {
    width: 100% !important; /* Importante para asegurar que ocupa el tamaño del contenedor */
    height: 100% !important; /* Importante para asegurar que ocupa el tamaño del contenedor */
  }

  .titulo-con-fondo {
    background-color: #17a2b8; /* Usa un color vivo de tu elección */
    color: white; /* Color del texto */
    padding: 10px 20px; /* Ajusta el relleno a tu gusto */
    border-radius: 50px; /* Esto crea bordes redondeados. Ajusta para cambiar la forma */
    display: inline-block; /* Esto permite que el fondo se ajuste al texto */
    font-size: 1.5em; /* Ajusta el tamaño del texto según sea necesario */
    font-family: Arial, sans-serif; /* Elige una tipografía apropiada */
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2); /* Opcional: Añade una sombra para resaltar el título */
  }

  /*clase de div centrado*/
  .Información {
    display: flex;
    justify-content: center; /* Centra horizontalmente */
    align-items: center; /* Centra verticalmente */
  }
</style>

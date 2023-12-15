<script>
  import { onMount, onDestroy } from "svelte";
  import Chart from "chart.js/auto";

  export let codigo = "";

  let resultadosEstudiante = null;
  let isLoading = false;
  let error = null;
  let myChart;

  const cargarResultados = async () => {
    isLoading = true;
    error = null;
    try {
      const response = await fetch(
        `http://localhost:8080/api/estudiantes/${codigo}/resultados`
      );
      if (!response.ok) {
        throw new Error("Error en la respuesta de la API");
      }
      resultadosEstudiante = await response.json();
    } catch (e) {
      error =
        e.message ||
        "No se encontraron resultados con el código del estudiante proporcionado";
    } finally {
      isLoading = false;
    }
  };

  // Reactiva: se ejecuta cada vez que cambia 'codigo'
  $: cargarResultados(), codigo;

  let canvasElement;

  // Reactiva: inicializa el gráfico una vez que los datos y el canvas están disponibles
  $: {
    if (resultadosEstudiante && canvasElement) {
      inicializarGrafico();
    }
  }

  function inicializarGrafico() {
    if (myChart) {
      myChart.destroy(); // Destruye la instancia anterior del gráfico si existe
    }

    const ctx = canvasElement.getContext("2d");
    // Aquí procesarías 'resultadosEstudiante' para obtener los datos del gráfico
    const datosGrafico = procesarDatos(resultadosEstudiante);

    myChart = new Chart(ctx, {
      type: "bar",
      data: {
        labels: [
          "Comunicación",
          "Razonamiento",
          "Lectura",
          "Ciudadanas",
          "Inglés",
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

  function procesarDatos(resultados) {
    // Inicializar sumas
    let sumaComunicacion = 0;
    let sumaRazonamiento = 0;
    let sumaLectura = 0;
    let sumaCiudadanas = 0;
    let sumaIngles = 0;

    // Iterar sobre cada objeto en el arreglo
    resultados.forEach((resultado) => {
      sumaComunicacion += resultado.comunicacionExamen;
      sumaRazonamiento += resultado.razonamientoExamen;
      sumaLectura += resultado.lecturaExamen;
      sumaCiudadanas += resultado.ciudadanasExamen;
      sumaIngles += resultado.inglesExamen;
    });

    // Calcular promedios
    let promedioComunicacion = sumaComunicacion / resultados.length;
    let promedioRazonamiento = sumaRazonamiento / resultados.length;
    let promedioLectura = sumaLectura / resultados.length;
    let promedioCiudadanas = sumaCiudadanas / resultados.length;
    let promedioIngles = sumaIngles / resultados.length;

    // Devolver un arreglo con los promedios
    return [
      promedioComunicacion,
      promedioRazonamiento,
      promedioLectura,
      promedioCiudadanas,
      promedioIngles,
    ];
  }

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
{:else if resultadosEstudiante}
  <div class="alert alert-success">
    <pre>{JSON.stringify(resultadosEstudiante, null, 2)}</pre>
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
</style>

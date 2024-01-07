<script>
  import { onDestroy } from "svelte";
  import Chart from "chart.js/auto";

  export let resultadosFinales = null;
  let error = "";
  let myChart;

  //Metodo para cargar los resultados del estudiante
  const cargarResultados = async () => {};

  let canvasElement;

  // Reactiva: inicializa el gráfico una vez que los datos y el canvas están disponibles
  $: {
    if (resultadosFinales && canvasElement) {
      inicializarGrafico();
    }
  }

  function inicializarGrafico() {
    if (myChart) {
      myChart.destroy(); // Destruye la instancia anterior del gráfico si existe
    }

    const ctx = canvasElement.getContext("2d");

    myChart = new Chart(ctx, {
      type: "bar",
      data: {
        labels: [
          "PromedioLectura",
          "PromedioMatematicas",
          "PromedioSociales",
          "PromedioNaturales",
          "PromedioIngles",
        ],
        datasets: [
          {
            label: "Calificación Promedio por Sección",
            data: resultadosFinales,
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

  onDestroy(() => {
    if (myChart) {
      myChart.destroy();
    }
  });
</script>

{#if error}
  <div class="alert alert-danger">{error}</div>
{:else if resultadosFinales}
  <div class="Información">
    <h1 class="titulo-con-fondo">Valor Agregado</h1>
  </div>
  <div class="chart-container">
    <canvas bind:this={canvasElement} />
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

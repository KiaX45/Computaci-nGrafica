<script>
  import { onMount, onDestroy } from "svelte";
  import Chart from "chart.js/auto";
  import { createEventDispatcher } from 'svelte';

  export let codigo = "";
  let dispatch = createEventDispatcher();
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
      //error = e.message || "No se encontraron resultados con el código del estudiante proporcionado";
        // "No se encontraron resultados con el código del estudiante proporcionado";
        //ponemos resultados por defecto para testear
        //TODO: eliminar los datos 
        resultadosEstudiante = [
          {
            codigoEstudiante: "2018100001",
            nombreEstudiante: "Juan",
            apellidoEstudiante: "Perez",
            comunicacionExamen: 140,
            razonamientoExamen: 250,
            lecturaExamen: 270,
            ciudadanasExamen: 122,
            inglesExamen: 190,
          }
        ];
        dispatch('enviarDatos', resultadosEstudiante);
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

  let studentName = "";
  let studentCode = "";
  function procesarDatos(resultados) {
    // Inicializar sumas
    let sumaComunicacion = 0;
    let sumaRazonamiento = 0;
    let sumaLectura = 0;
    let sumaCiudadanas = 0;
    let sumaIngles = 0;

    // Iterar sobre cada objeto en el arreglo
    resultados.forEach((resultado) => {
      studentCode = resultado.codigoEstudiante;
      studentName =
        resultado.nombreEstudiante + " " + resultado.apellidoEstudiante;
      // Sumar cada nota
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

    //creamos un objeto que lamacene los resultados calculados
    let resultadosFinales= {
      promedioComunicacion,
      promedioRazonamiento,
      promedioLectura,
      promedioCiudadanas,
      promedioIngles,
    }

    dispatch('enviarDatos', resultadosFinales);


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

<div class="main-Container">
  <div class="Información estudiante"></div>

  {#if isLoading}
    <p>Cargando...</p>
  {:else if error}
    <div class="alert alert-danger">{error}</div>
  {:else if resultadosEstudiante}
    <!-- Tarjeta de Información del Estudiante -->
    <div class="student-card">
      <div class="student-avatar">
        <!-- Reemplaza la URL del src con la ruta de tu imagen de avatar genérica -->
        <img
          src="https://thumbs.dreamstime.com/z/ilustraci%C3%B3n-de-avatar-estudiante-icono-perfil-usuario-juvenil-retrato-simple-un-dibujos-animados-vectorial-276205546.jpg"
          alt="Avatar del Estudiante"
        />
      </div>
      <div class="student-info">
        <div class="student-name">Nombre: {studentName}</div>
        <div class="student-id">Código: {studentCode}</div>
        <div class="student-semester">Semestre: 6</div>
      </div>
    </div>
    <div class="Información">
      <h1 class="titulo-con-fondo">Simulacros</h1>
    </div>

    <div class="chart-container">
      <canvas bind:this={canvasElement}></canvas>
    </div>
  {/if}
</div>

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

  .main-Container {
    margin: auto;
    align-items: center;
  }

  .student-card {
    display: flex;
    background-color: #f2f2f2;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    padding: 16px;
    align-items: center;
    margin: 16px;
  }

  .student-avatar {
    flex: 0 0 100px; /* ajusta el tamaño según tus necesidades */
    padding-right: 16px; /* espacio entre el avatar y la información */
  }

  .student-avatar img {
    width: 100%; /* hace que la imagen del avatar sea completamente responsive dentro del div */
    border-radius: 50%; /* crea una imagen redonda */
  }

  .student-info {
    flex: 1;
    text-align: left;
  }

  .student-name,
  .student-id,
  .student-semester {
    margin-bottom: 8px;
    font-size: 1rem;
    color: #333;
  }

  /* Puedes añadir media queries para hacer la tarjeta responsiva */
  @media (max-width: 768px) {
    .student-card {
      flex-direction: column;
      text-align: center;
    }

    .student-avatar {
      padding-right: 0;
      padding-bottom: 16px;
    }
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

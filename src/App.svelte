<script>
	//importamos lo necesario para poder hacer el routing
	import { Router, Link, Route } from "svelte-routing";

	//importamos los componentes que vamos a usar
	import PlanearExamen from "./components/admin/views/PlanearExamen.svelte";
	//importamos componenete para subir notas
	import SubirNotas from "./components/admin/views/SubirNotasExamenes.svelte";
	//importamos componente estadisticas
	import Estadisticas from "./components/admin/views/EstadisticasExamen.svelte";
	//importamos componente para planear actividades
	import PlanearActividades from "./components/admin/views/PlaneatrActividades.svelte";
</script>

<body>
	<!--Hacemos el routing dentro de un navegador-->
	<Router>
		<nav class="navbar navbar-expand-lg">
			<ul>
				<div class="container text-center">
					<div class="row">
						<div class="col-md-auto">
							<li><Link to="/estadisticas">Estadisticas</Link></li>
						</div>
						<div class="col-md-auto">
							<li><Link to="/planearExamen">Planear Examen</Link></li>
						</div>
						<div class="col-md-auto">
							<li><Link to="/subirNotas">Subir Notas</Link></li>
						</div>
						<div class="col-md-auto">
							<li><Link to="/planearActividades">Planear Actividades</Link></li>
						</div>
					</div>
				</div>
			</ul>
		</nav>

		<Route path="/planearExamen" component={PlanearExamen} />
		<Route path="/subirNotas" component={SubirNotas} />
		<Route path="/estadisticas" component={Estadisticas} />
		<Route path="/planearActividades" component={PlanearActividades} />
	</Router>
</body>

<style>
	body {
    margin: 0;
    padding: 0;
  }

  .navbar {
    background-color: rgba(255, 255, 255, 0.6); /* Fondo del navbar con opacidad */
    /* Puedes ajustar otros estilos del navbar aquí */
  }

  .navbar ul {
    list-style-type: none;
    margin: 0;
    padding: 0;
  }

  .navbar ul li {
    display: inline;
    margin-right: 10px; /* Espaciado entre elementos del navbar */
  }

  /* Añadir el fondo solo al navbar */
  .navbar {
    background-image: url("https://situr.narino.gov.co/storage/Clientes/situr_narino/principal/imagenes/contenidos/7957-22_Universidad_de_Nari%C3%B1o_Academia_de_Historia.jpg");
    background-size: cover; /* Ajusta el tamaño de la imagen al contenedor */
    background-repeat: no-repeat; /* Evita la repetición de la imagen de fondo */
    background-position: center center; /* Centra la imagen en el fondo */
  }
</style>

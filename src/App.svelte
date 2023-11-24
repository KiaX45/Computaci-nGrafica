<script>
//importamos lo necesario para poder hacer el routing
import { Router, Link, Route } from "svelte-routing";

//importamos los componentes que vamos a usar
import PlanearExamen from "./components/admin/views/PlanearExamen.svelte";
//importamos componenete para subir notas
import SubirNotas from "./components/admin/views/SubirNotasExamenes.svelte";

</script>

<!--Hacemos el routing dentro de un navegador-->
<Router>

	<nav class="navbar navbar-expand-lg bg-body-tertiary">
		<div class="container-fluid">
			<a class="navbar-brand" href="#">UDENAR</a>
			<button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNavAltMarkup" aria-controls="navbarNavAltMarkup" aria-expanded="false" aria-label="Toggle navigation">
				<span class="navbar-toggler-icon"></span>
			</button>
			<div class="collapse navbar-collapse" id="navbarNavAltMarkup">
				<div class="navbar-nav">
					<a class="nav-link active" aria-current="page" href="#">Home</a>
					<a class="nav-link" href="/planearExamen">Planeación</a>
					<a class="nav-link" href="/subirNotas">Seguimiento</a>
				</div>
			</div>
		</div>
	</nav>

  <Route path="/planearExamen" component={PlanearExamen} />
  <Route path="/subirNotas" component={SubirNotas} />
</Router>

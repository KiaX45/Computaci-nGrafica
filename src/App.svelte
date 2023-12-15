<script>
//importamos lo necesario para poder hacer el routing
import { Router, Link, Route } from "svelte-routing";

//importamos los componentes que vamos a usar
import PlanearExamen from "./components/admin/views/PlanearExamen.svelte";
//importamos componenete para subir notas
import SubirNotas from "./components/admin/views/SubirNotasExamenes.svelte";
//importamos componente estadisticas
import Estadisticas from "./components/admin/views/EstadisticasExamen.svelte";

</script>

<!--Hacemos el routing dentro de un navegador-->
<Router>
	<nav class="navbar navbar-expand-lg bg-body-tertiary">
		<ul>
				<li><Link to="/estadisticas">Estadisticas</Link></li>
				<li><Link to="/planearExamen">Planear Examen</Link></li>
				<li><Link to="/subirNotas">Subir Notas</Link></li>
		</ul>
</nav>

  <Route path="/planearExamen" component={PlanearExamen} />
  <Route path="/subirNotas" component={SubirNotas} />
  <Route path="/estadisticas" component={Estadisticas} />
</Router>

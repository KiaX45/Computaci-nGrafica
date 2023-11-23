<script>
//importamos lo necesario para poder hacer el routing
import { Router, Link, Route } from "svelte-routing";

//importamos los componentes que vamos a usar
import PlanearExamen from "./components/admin/views/PlanearExamen.svelte";
//importamos componenete para subir notas
import SubirNotas from "./components/admin/views/SubirNotasExamenes.svelte";

</script>

<main>
<div>

  <h1>Universidad de Nariño</h1>
  <Router>
    <nav>
      <Link to="/planearExamen">Planear Examen</Link>
      <Link to="/subirNotas">Subir Notas</Link>
    </nav>

    <Route path="/planearExamen" component={PlanearExamen} />
    <Route path="/subirNotas" component={SubirNotas} />
  </Router>

</div>


</main>

<style>
  h1 {
   color: #000;
  }
</style>
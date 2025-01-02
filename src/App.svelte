<script>
  import { Router, Route, Link } from 'svelte-routing';
  import Login from './Login.svelte';
  import Register from './Register.svelte';
</script>
<Router>
  <nav>
    <Link to="/login">Login</Link> | <Link to="/register">Register</Link>
  </nav>

  <Route path="/login" component={Login} />
  <Route path="/register" component={Register} />
</Router>
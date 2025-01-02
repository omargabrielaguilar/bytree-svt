<!-- src/Login.svelte -->
<script>
  let email = '';
  let password = '';
  let errorMessage = '';

  async function login() {
    try {
      const response = await fetch('http://localhost:3000/auth/login', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({ email, password }),
      });

      if (!response.ok) {
        const errorData = await response.json();
        errorMessage = errorData.message || 'Error en el login';
        return;
      }

      const data = await response.json();
      localStorage.setItem('access_token', data.access_token);  // Guardamos el token
      window.location.href = '/';  // Redirigimos después del login
    } catch (error) {
      errorMessage = 'Ocurrió un error inesperado.';
    }
  }
</script>

<div class="min-h-screen flex justify-center items-center bg-gray-100">
  <div class="bg-white p-8 rounded-lg shadow-lg w-full max-w-sm">
    <h2 class="text-2xl font-bold mb-6 text-center">Iniciar sesión</h2>
    <form on:submit|preventDefault={login}>
      <div class="mb-4">
        <label for="email" class="block text-sm font-medium text-gray-700">Email</label>
        <input type="email" id="email" bind:value={email} required class="w-full p-3 mt-1 border border-gray-300 rounded-lg focus:ring-blue-500 focus:border-blue-500" />
      </div>
      <div class="mb-6">
        <label for="password" class="block text-sm font-medium text-gray-700">Contraseña</label>
        <input type="password" id="password" bind:value={password} required class="w-full p-3 mt-1 border border-gray-300 rounded-lg focus:ring-blue-500 focus:border-blue-500" />
      </div>
      <button type="submit" class="w-full bg-blue-500 text-white p-3 rounded-lg hover:bg-blue-600">Iniciar sesión</button>
    </form>

    {#if errorMessage}
      <div class="mt-4 text-red-500 text-center">{errorMessage}</div>
    {/if}
  </div>
</div>

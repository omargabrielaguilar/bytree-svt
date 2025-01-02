<!-- src/Register.svelte -->
<script>
  let email = '';
  let password = '';
  let confirmPassword = '';
  let errorMessage = '';

  async function register() {
    if (password !== confirmPassword) {
      errorMessage = 'Las contraseñas no coinciden';
      return;
    }

    try {
      const response = await fetch('http://localhost:3000/auth/register', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({ email, password, confirmPassword }),
      });

      if (!response.ok) {
        const errorData = await response.json();
        errorMessage = errorData.message || 'Error en el registro';
        return;
      }

      window.location.href = '/login';  // Redirigimos al login después del registro
    } catch (error) {
      errorMessage = 'Ocurrió un error inesperado.';
    }
  }
</script>

<div class="min-h-screen flex justify-center items-center bg-gray-100">
  <div class="bg-white p-8 rounded-lg shadow-lg w-full max-w-sm">
    <h2 class="text-2xl font-bold mb-6 text-center">Crear cuenta</h2>
    <form on:submit|preventDefault={register}>
      <div class="mb-4">
        <label for="email" class="block text-sm font-medium text-gray-700">Email</label>
        <input type="email" id="email" bind:value={email} required class="w-full p-3 mt-1 border border-gray-300 rounded-lg focus:ring-blue-500 focus:border-blue-500" />
      </div>
      <div class="mb-4">
        <label for="password" class="block text-sm font-medium text-gray-700">Contraseña</label>
        <input type="password" id="password" bind:value={password} required class="w-full p-3 mt-1 border border-gray-300 rounded-lg focus:ring-blue-500 focus:border-blue-500" />
      </div>
      <div class="mb-6">
        <label for="confirmPassword" class="block text-sm font-medium text-gray-700">Confirmar Contraseña</label>
        <input type="password" id="confirmPassword" bind:value={confirmPassword} required class="w-full p-3 mt-1 border border-gray-300 rounded-lg focus:ring-blue-500 focus:border-blue-500" />
      </div>
      <button type="submit" class="w-full bg-green-500 text-white p-3 rounded-lg hover:bg-green-600">Registrar</button>
    </form>

    {#if errorMessage}
      <div class="mt-4 text-red-500 text-center">{errorMessage}</div>
    {/if}
  </div>
</div>

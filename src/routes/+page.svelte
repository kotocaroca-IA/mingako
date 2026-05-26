<script lang="ts">
  import './mingako-landing.css';
  import { onMount } from 'svelte';
  
  let emailValue = $state('');
  let isRegistered = $state(false);
  let isLoading = $state(false);
  let errorMessage = $state('');
  let instantSuccess = $state(false);

  onMount(() => {
    const saved = localStorage.getItem('mingako_notified_email');
    if (saved) {
      isRegistered = true;
      instantSuccess = true;
    }
  });

  function validateEmail(email: string) {
    const re = /^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/;
    return re.test(email);
  }

  function handleInput() {
    if (errorMessage) {
      errorMessage = '';
    }
  }

  async function handleSubmit(e: SubmitEvent) {
    e.preventDefault();
    const email = emailValue.trim();

    if (!validateEmail(email)) {
      errorMessage = 'Ingresa un correo válido.';
      return;
    }

    isLoading = true;
    errorMessage = '';

    try {
      await new Promise(resolve => setTimeout(resolve, 1200));
      localStorage.setItem('mingako_notified_email', email);
      isLoading = false;
      isRegistered = true;
      instantSuccess = false;
    } catch (err) {
      isLoading = false;
      errorMessage = 'Ocurrió un problema. Por favor inténtalo de nuevo.';
    }
  }

  function handleReset() {
    localStorage.removeItem('mingako_notified_email');
    isRegistered = false;
    emailValue = '';
    errorMessage = '';
    instantSuccess = false;
  }
</script>

<svelte:head>
  <!-- SEO Meta Tags -->
  <title>Mingako | Consciencia Ambiental Colectiva - Muy Pronto</title>
  <meta name="description" content="Estamos construyendo un nuevo sitio web para Mingako. Déjanos tu correo y te avisaremos cuando esté listo. Consciencia ambiental colectiva.">
  <meta name="keywords" content="mingako, consciencia ambiental, colectiva, sustentabilidad, medio ambiente, ecologia, reciclaje, compostaje">
  <meta name="author" content="Mingako">
  
  <!-- Open Graph / Facebook -->
  <meta property="og:type" content="website">
  <meta property="og:title" content="Mingako | Consciencia Ambiental Colectiva - Muy Pronto">
  <meta property="og:description" content="Estamos construyendo un nuevo sitio web para Mingako. Déjanos tu correo y te avisaremos cuando esté listo.">
  <meta property="og:image" content="/assets/background.webp">

  <!-- Twitter -->
  <meta property="twitter:card" content="summary_large_image">
  <meta property="twitter:title" content="Mingako | Consciencia Ambiental Colectiva - Muy Pronto">
  <meta property="twitter:description" content="Estamos construyendo un nuevo sitio web para Mingako. Déjanos tu correo y te avisaremos cuando esté listo.">
  <meta property="twitter:image" content="/assets/background.webp">

  <!-- Fonts -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="anonymous">
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;700;800&family=Space+Grotesk:wght@500;700&display=swap" rel="stylesheet">
</svelte:head>

<!-- Background Wrapper -->
<div class="page-container">
  
  <!-- Main Content Container -->
  <main class="content-wrapper">
    
    <!-- Logo Header -->
    <header class="logo-header">
      <img src="/assets/logo-mingako.svg" alt="Mingako - Consciencia Ambiental Colectiva" class="logo-img">
    </header>

    <!-- Central Under Construction Panel -->
    <section class="panel-container">
      
      <!-- Status / Headings -->
      <div class="panel-headings">
        <span class="status-badge">Próximamente</span>
        <h1 class="main-title">
          Estamos construyendo<br>un nuevo sitio web.
        </h1>
      </div>

      {#if !isRegistered}
        <!-- Form and Newsletter -->
        <div class="newsletter-container">
          <p class="newsletter-sub">
            Ingresa tu mejor correo y te avisaremos cuando esté listo.
          </p>
          
          <form class="newsletter-form" onsubmit={handleSubmit}>
            <div class="input-wrapper">
              <input 
                type="email" 
                bind:value={emailValue}
                oninput={handleInput}
                id="emailInput" 
                placeholder="Ingresa tu correo aquí" 
                required 
                aria-label="Correo electrónico"
                autocomplete="email"
                style:border-color={errorMessage ? '#c2410c' : ''}
                disabled={isLoading}
              >
              {#if errorMessage}
                <span class="error-msg visible" aria-live="polite">{errorMessage}</span>
              {/if}
            </div>
            <button type="submit" class="btn-submit" class:loading={isLoading} disabled={isLoading}>
              <span class="btn-text">Notificarme</span>
              <div class="btn-loader" aria-hidden="true"></div>
            </button>
          </form>
        </div>
      {:else}
        <!-- Success Message -->
        <div class="success-container" style="display: flex; opacity: 1; {instantSuccess ? 'transform: scale(1); animation: none;' : ''}" aria-hidden="false">
          <div class="success-check" aria-hidden="true">✓</div>
          <p class="success-text">Listo, te avisaremos cuando este sitio esté listo</p>
          <button class="success-reset" onclick={handleReset} type="button">ingresar otro correo</button>
        </div>
      {/if}

      <!-- Social Links -->
      <footer class="social-footer">
        <a href="https://www.instagram.com/mingako_organico/" target="_blank" rel="noopener noreferrer" class="social-link" aria-label="Síguenos en Instagram">
          <img src="/assets/link-instagram.svg" class="social-icon" alt="Instagram">
        </a>
        <a href="https://www.facebook.com/mingako.cl" target="_blank" rel="noopener noreferrer" class="social-link" aria-label="Síguenos en Facebook">
          <img src="/assets/link-facebook.svg" class="social-icon" alt="Facebook">
        </a>
        <a href="https://www.youtube.com/@mingakocongresodeeducaciona2093" target="_blank" rel="noopener noreferrer" class="social-link" aria-label="Suscríbete a nuestro canal de YouTube">
          <img src="/assets/link-youtube.svg" class="social-icon" alt="YouTube">
        </a>
      </footer>

    </section>

  </main>

  <!-- Footnote Sello Green Hosting -->
  <div class="hosting-badge">
    <a href="https://www.infomaniak.com" target="_blank" rel="noopener noreferrer" aria-label="Alojado en Infomaniak - Green Hosting">
      <img src="/assets/green-hosting.png" alt="Sello Green Hosting - Hosted by Infomaniak - Green Web Foundation" class="hosting-img">
    </a>
  </div>

</div>

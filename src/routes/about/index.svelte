<script context="module">
  import { register, waitLocale } from 'svelte-i18n';

  register('en', () => import('./messages/en.json'));
  register('pt-BR', () => import('./messages/pt-BR.json'));
  register('es-ES', () => import('./messages/es-ES.json'));
  register('ar', () => import('./messages/ar.json'));

  export function preload() {
    return waitLocale();
  }
</script>

<script>
  import { _ } from 'svelte-i18n';
</script>

<style>
  .rtl {
    direction: rtl;
  }
</style>

<svelte:head>
  <title>{$_('title.about')}</title>
</svelte:head>

<div class={$_('direction')}>
  <h1>{$_('about_this_site')}</h1>
  <p>{$_('about_content.0')}</p>
</div>

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
  <title>{$_('nav.routing')}</title>
</svelte:head>

<div class={$_('direction')}>
  <h1>{$_('routing.title')}</h1>
  <h2>Regular expressions in the routing</h2>
  <p>In order to make it so that a file can be resolved by a path like: 
    <code>/about</code> AND <code>/es-ES/about</code>
      We put the following in the folder name: <code>[...a(.*about)]</code>.
      This is a regular expression.
  </p>
  <dl>
    <dt>...a()</dt>
    <dd>The group (what is enclosed in brackets () ) should be assigned to the 'a' variable. We use the ... spread operator so that it's split on '/'</dd>
    <dt>.*about</dt>
    <dd>Capture: . (any character), * (repeated 0 or more times), followed by 'about'</dd>
    <dd>Alternatively we can write: <code>[...i(.&lbrace;0&rbrace;|es-ES|pt-BR|ar)]</code></dd>
  </dl>
    
  <h2>Important Caveats</h2>
  <p>You MUST use a different variable: i.e.<code>...a()</code>, <code>...b()</code>etc. for each path, or they will override each other and you will pull your hair out.</p>
  <p>You can't use anything in these regular expressions. <code>(, ), ?, :, /, \</code> are not allowed characters (as they are normally in regular expressions), so this is one of the few combinations that worked.</p>
  



</div>

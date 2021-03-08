<script context="module">
  import { isLoading, waitLocale } from 'svelte-i18n';

  export async function preload({ params, query }) {
    console.log("params", params)
    waitLocale();
    return { page: params }
  }
</script>


<script>
  import { goto, stores } from '@sapper/app';
  import { _, locale, locales } from 'svelte-i18n';
  export let segment;
  
  const { page } = stores();

  let localePath = $locale === "en" ? "" : $locale

  const setLocale = (item) => {
    let [, lang, ...rest] = $page.path.split('/')

    let pagePart = [lang, ...rest].join('/')
    if ($locales.includes(lang)) {
      pagePart = rest.join('/')
    } else {
      console.log("no language is included")
    }

    $locale = item
    localePath = $locale === "en" ? "" : $locale + '/'
    goto(localePath + pagePart)
  }

</script>

<style>
  nav {
    border-bottom: 1px solid rgba(255, 62, 0, 0.1);
    font-weight: 300;
    padding: 0 1em;
    display: flex;
    justify-content: space-between;
  }

  ul {
    margin: 0;
    padding: 0;
  }

  /* clearfix */
  ul::after {
    content: '';
    display: block;
    clear: both;
  }

  li {
    display: block;
    float: left;
  }

  .selected {
    position: relative;
    display: inline-block;
  }

  .selected::after {
    position: absolute;
    content: '';
    width: calc(100% - 1em);
    height: 2px;
    background-color: rgb(255, 62, 0);
    display: block;
    bottom: -1px;
  }

  a,
  .a {
    cursor: pointer;
    text-decoration: none;
    padding: 1em 0.5em;
    display: block;
  }
  .rtl {
    direction: rtl;
    display: flex;
  }
</style>

<nav class={$_('direction')}>
  <ul class={$_('direction')}>
    <li>
      <a class:selected={segment === undefined} href="{localePath}">{$_('nav.home')}</a>
    </li>
    <li>
      <a class:selected={segment === 'about'} href="{localePath}about">{$_('nav.about')}</a>
    </li>
    <li>
      <a class:selected={segment === 'routing'} href="{localePath}routing">{$_('nav.routing')}</a>
    </li>
  </ul>
  <ul class="lang">
    {#each $locales as item}
      <li>
        <span
          class="a"
          class:selected={$locale.includes(item)}
          href={`#!${item}`}
          on:click={setLocale(item)}>
          {$_('languages.' + item.replace('-', '_'))}
        </span>
      </li>
    {/each}
  </ul>
</nav>

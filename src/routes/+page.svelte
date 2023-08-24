<script>
  import Hero from "$lib/Hero.svelte";
  import Produtos from "$lib/Produtos.svelte";

  export let data;
  console.log(data);

</script>

<div class="painel">
  
<Hero/>

</div>

<Produtos/>

<style>
  .painel{
    position: relative;
  }
</style>


<script>
  import Marca from "$lib/assets/Marca.svelte";
  import { X, PlusCircle, MinusCircle } from 'lucide-svelte';
  import {modal, lista} from '$lib/stores'
  import toast, { Toaster } from 'svelte-french-toast';

  $: nome = $modal.nome;
  $: preco = $modal.preco;
  $: metrica = $modal.metrica;
  $: img = $modal.img;
  $: alt = $modal.alt;
  let quantidade = 1;

  // fechar modal
  function fecharModal() {
    $modal.aberto = false;
    quantidade = 1;
  }

  // mostrar qual tecla foi pressionada
  function teclaPressionada(evento) {
    if (evento.key === 'Escape'){
        fecharModal();
    }
  }

  // alterar quantidade
  function aumentar() {
    quantidade++;
  }
  function diminuir() {
    if (quantidade <= 1) return;
    quantidade--;
  }

  // adicionar produto ao pedido
  function adicionarProduto() {
    const produtoExiste = $lista.produtos.findIndex((produto) => produto.nome === nome);
    

    // caso exista, aumentar quantidade
    if (produtoExiste !== -1) {
      $lista.produtos[produtoExiste].quantidade += quantidade;
    }
    // caso não exista, adicionar novo produto
    if (produtoExiste === -1){
      
      $lista.produtos = [...$lista.produtos,
        {
          nome,
          quantidade,
          preco,
          metrica
        }
      ]
    }
    toast.success("Produto adicionado!", {
      position: "top-right"
    });
  }
  
</script>

<Toaster/>

<svelte:window on:keyup={teclaPressionada} />

{#if $modal.aberto}
<section class="modal">
  <div class="modal__bg" on:click={fecharModal}  aria-hidden="true"></div>
  <div class="modal__container container">
    <div class="card">
      <div class="imagem__container">
        <img src={img} alt={alt}>
      </div>

      <div class="conteudo">
          <h3 class="nome">{nome}</h3>

        <div class="info">
          <span class="preco">{preco}</span>
          <span class="separador"></span>
          <span class="metrica">{metrica}</span>
        </div>

        <div class="quantidade__container">
          <label for="quantidade">Quantidade</label>
          <input bind:value={quantidade} type="number" name="quantidade" id="quantidade">
          <div class="btns">
            <button on:click={aumentar}><PlusCircle/></button>
            <button on:click={diminuir}><MinusCircle/></button>
          </div>
          <button on:click={adicionarProduto} class="btn">Adicionar ao pedido</button>
        </div>

        <div class="marca">
          <Marca/>
        </div>
      </div>

      <button on:click={fecharModal} class="fechar" title="Fechar"><X/></button>
    </div>
  </div>
</section>
{/if}


<style>
  .modal {
    position: fixed;
    width: 100%;
    height: 100%;
    top: 0;
    display: flex;
  }
  .modal__bg {
    position: absolute;
    inset: 0;
    background: var(--modal);
    backdrop-filter: blur(4px);
  }
  .card {
    max-width: 600px;
    overflow: hidden;
    position: relative;
    z-index: 1;
  }
  .nome {
    font-size: calc(24 / 16 * 1rem);
  }
  .conteudo {
    padding: 24px;
    padding-bottom: 36px;
    background-color: var(--bg);
    position: relative;
  }
  .info {
    margin-top: 8px;
    display: flex;
    align-items: center;
    gap: 8px;
  }
  .preco {
    color: #fff;
    font-size: calc(14 / 16 * 1rem);
    background-color: var(--destaque);
    padding: 2px 8px;
    border-radius: 16px;
  }
  .separador {
    width: 36px;
    height: 2px;
    background-color: var(--bg-2);
  }
  .metrica {
    font-size: calc(14 / 16 * 1rem);
  }
  .quantidade__container {
    margin-top: 24px;
    display: flex;
    flex-wrap: wrap;
    gap: 4px 8px;
    grid-template-columns: repeat(3, 1fr);
    justify-content: start;
    align-items: center;
  }
  label {
    font-size: calc(14 / 16 * 1rem);
    grid-column: span 3;
    flex-basis: 100%;
  }
  input {
    font-size: calc(14 / 16 * 1rem);
    color: var(--principal);
    border: 2px solid var(--bg-2);
    background-color: transparent;
    cursor: pointer;
    text-align: center;
    height: 40px;
    width: 40px;
  }
  input::placeholder {
    color: var(--principal);
  }
  input[type=number]::-webkit-inner-spin-button {
    -webkit-appearance: none;
  }
  .btn {
    font-size: calc(14 / 16 * 1rem);
    background-color: var(--bg-2);
    padding: 12px 16px;
    margin-left: 16px;
    width: fit-content;
    transition: 125ms;
  }
  .btn:hover {
    background-color: var(--bg-hover);
  }
  .marca {
    position: absolute;
    right: 16px;
    bottom: 16px;
  }
  .fechar {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 40px;
    height: 40px;
    background-color: var(--bg-2);
    position: absolute;
    top: 16px;
    right: 16px;
    transition: 125ms;
  }
  .fechar:hover {
    background-color: var(--bg-hover);
  }
  .imagem__container {
    position: relative;
  }
  .imagem__container img {
    aspect-ratio: 16/9;
    height: 400px;
    object-fit: cover;
    object-position: center;
  }
  .btns {
    display: flex;
    gap: 2px;
    flex-direction: column;
  }
</style>
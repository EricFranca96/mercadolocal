<footer class="footer">
  <div class="footer__container container">
    <p>© Mercado Local, 2023</p>
    <p>Desenvolvido por <a href="#" > Eric França</a></p>
  </div>
</footer>

<style>
  .footer__container {
    display: flex;
    align-items: flex-start;
    justify-content: space-between;
    padding-top: 24px;
    padding-bottom: 24px;
    flex-wrap: wrap;
    gap: 8px;
  }
  p {
    font-size: calc(14 / 16 * 1rem);
    color: var(--principal-2);
  }
  p a {
    color: var(--principal-2);
  }
  p a:hover {
    text-decoration: underline;
  }
</style>
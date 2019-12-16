<script>
  import DropZone from "./components/DropZone.svelte";
  import Help from "./components/Help.svelte";
</script>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }

  h1 {
    color: #000;
    text-transform: uppercase;
    font-size: 4em;
    font-weight: 100;
  }

  p {
    font-size: 2em;
    font-weight: 200;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>

<main>
  <h1>CSV-TO-QRCODE</h1>
  <p>Make a Qrcode so easy!</p>
  <DropZone />
  <Help />
</main>

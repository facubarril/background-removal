<script lang="ts">
  import "two-up-element"
  import { originalImage } from "./store";
</script>

<two-up>
  <img src={$originalImage} alt="Imagen orignal" class="max-h-96 max-w-64">
  <img src="" alt="Imagen editada" class="max-h-96 max-w-64">
</two-up>

<a href="" class="bg-blue-500 hover:bg-blue-700 text-xl text-center w-full font-bold text-white rounder-full px-4 py-2 mt-10" download>Descargar imagen sin fondo</a>
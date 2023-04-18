<script lang="ts">
  import "two-up-element"
  import { originalImage, modifiedImage } from "./store";

  let processingImage = true
  let tries = 0
  let intervalId : any

  $: {
    if (processingImage) {
      clearInterval(intervalId)
      intervalId = setInterval(() => {
        tries++
      }, 500)
    }
  }
</script>

<two-up>
  <img src={ $originalImage } alt="Imagen orignal" class="max-h-148 object-cover w-full">
  <img
    on:load={ () => (processingImage = false) }
    on:error={ () => (processingImage = true) }
    src={`${$modifiedImage}&t=${tries}`}
    alt="Imagen editada"
    class="max-h-148 object-cover w-full"
  >
</two-up>

<a href={ $modifiedImage } class="block bg-blue-500 hover:bg-blue-700 text-xl text-center w-full font-bold text-white rounded-full px-4 py-2 mt-4" download target="_blank">Descargar imagen sin fondo</a>
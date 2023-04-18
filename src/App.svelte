<script lang="ts">
  import { ImageStatus } from "../types.d";
  import StepEdit from "./StepEdit.svelte";
  import StepUpload from "./StepUpload.svelte";
  import ClaudinaryLogo from "./claudinaryLogo.svelte";
  import { imageStatus } from "./store";
</script>

<main class="max-w-xl m-auto grid grid-cols-1 gap-y-6 w-full h-screen p-4">
  <header class="flex justify-center py-10">
    <h1 class="text-3xl font-bold text-blue-900 tracking-tighter">
      Background <span class="text-blue-600">Removal</span>
    </h1>
  </header>

  <section>
    {#if $imageStatus === ImageStatus.READY || $imageStatus === ImageStatus.UPLOADING}
      <StepUpload />
    {:else if $imageStatus === ImageStatus.DONE}
      <StepEdit />
    {/if}
  </section>

  <footer class="flex flex-col justify-center items-center gap-y-4 font-semibold pt-10">
    <span class="flex flex-row justify-center items-center gap-x-2">
      Hecho con <a href="https://claudinary.com" target="_blank" rel="noopener noreferrer">
        <ClaudinaryLogo/></a>
    </span>
    <a href="https://webcreatio.net" target="_blank"><img src="https://webcreatio.net/public/by_webcreatio-black.svg" alt="by Webcreatio" class="w-32"></a>
  </footer>
</main>

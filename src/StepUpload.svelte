<script lang="ts">
  import { ImageStatus } from '../types.d';
  import { imageStatus, originalImage } from './store';
  import Dropzone from 'dropzone'
  import 'dropzone/dist/dropzone.css'

  import { onMount } from 'svelte';

  onMount(() => {
    const dropzone = new Dropzone('#dropzone', {
      uploadMultiple: false,
      acceptedFiles: '.jpg, .png, .webp',
      maxFiles: 1
    })

    dropzone.on('sending', (file, xhr, formData) => {
      imageStatus.set(ImageStatus.UPLOADING)

      formData.append('upload_preset', 'ml_default')
      formData.append('timestamp', (Date.now() / 1000))
      formData.append('api_key', 546222121714181)
    })

    dropzone.on('success', (file, response) => {
      const { secure_url: url } = response
      imageStatus.set(ImageStatus.DONE)
      originalImage.set(url)

      console.log(response)
    })

    dropzone.on('error', (file, response) => {
      console.log('HA IDO MAL')
      console.log(response)
    })
  })
</script>

<form id="dropzone" class="shadow-2xl border-dashed border-2 border-gray-300 rounded-lg aspect-ratio w-full flex items-center justfy-center flex-col" action="https://api.cloudinary.com/v1_1/derpdqfii/image/upload">
  {#if $imageStatus === ImageStatus.READY}
    <button class="pointer-events-none bg-blue-600 rounded-full text-bold text-wide text-xl px-6 py-4 mt-12 text-white">
      Upload files
    </button>
    <strong class="text-lg mt-4 mb-8 text-gray-800">or drop a file</strong>
  {/if}

  {#if $imageStatus === ImageStatus.UPLOADING}
  <strong class="text-lg mt-4 text-gray-800 my-12">Uploading file...</strong>
  {/if}
</form>
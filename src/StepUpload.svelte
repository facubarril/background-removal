<script lang="ts">
  import { Cloudinary } from '@cloudinary/url-gen'
  import { backgroundRemoval } from '@cloudinary/url-gen/actions/effect';

  import { ImageStatus } from '../types.d';
  import { imageStatus, modifiedImage, originalImage } from './store';
  import Dropzone from 'dropzone'
  import 'dropzone/dist/dropzone.css'

  import { onMount } from 'svelte';

  const cloudinary = new Cloudinary({
    cloud: {
      cloudName: 'derpdqfii', //drrobazjo
    },
    url: {
      secure: true
    }
  })

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
      formData.append('api_key', 546222121714181) // 726767953163323
    })

    dropzone.on('success', (file, response) => {
      const { public_id: publicId, secure_url: url } = response

      const imageWithoutBackground = cloudinary
                                      .image(publicId)
                                      .effect(backgroundRemoval())

      imageStatus.set(ImageStatus.DONE)
      modifiedImage.set(imageWithoutBackground.toURL())
      originalImage.set(url)
    })

    dropzone.on('error', (file, response) => {
      console.log('Error: ', response)
    })
  })
</script>

<form id="dropzone" class="shadow-2xl border-dashed border-2 border-gray-300 rounded-lg aspect-ratio w-full flex items-center justfy-center flex-col" action="https://api.cloudinary.com/v1_1/derpdqfii/image/upload"> <!--drrobazjo -->
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
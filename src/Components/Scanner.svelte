<script>
  import { onMount } from 'svelte';
  import jsQR from 'jsqr';

  let qrCodeData = "Decoded text";
  let url = "";
  let disable = true;
  let allow = false;
  export let primarycol ;
  export let secondarycol ;
  export let toggle ;

  onMount(() => {
    const uploadButton = document.getElementById('uploadButton');
    const fileInput = document.getElementById('fileInput');

    uploadButton.addEventListener('click', () => {
      fileInput.click();
    });

    fileInput.addEventListener('change', () => {
      const file = fileInput.files[0];

      if (file) {
        const reader = new FileReader();

        reader.onload = (e) => {
          const img = new Image();
          img.onload = () => {
            const canvas = document.createElement('canvas');
            canvas.width = img.width;
            canvas.height = img.height;
            const ctx = canvas.getContext('2d');
            ctx.drawImage(img, 0, 0);
            const imageData = ctx.getImageData(0, 0, img.width, img.height);

            try {
              const code = jsQR(imageData.data, imageData.width, imageData.height, {
                inversionAttempts: 'dontInvert'
              });

              if (code) {
                disable = false;
                qrCodeData = code.data;
                url = qrCodeData;
              } else {
                qrCodeData = "QR Code not found.";
              }
            } catch (error) {
              qrCodeData = "Error decoding QR Code.";
            }
          };

          img.src = e.target.result;
        };

        reader.readAsDataURL(file);
      }
    });
  });

  function copyToClipboard(text) {
    navigator.clipboard.writeText(text)
      .then(() => {
        alert('Text copied to clipboard!');
      })
      .catch(err => {
        console.error('Failed to copy: ', err);
      });
  }

  function navigateToNewPage() {
    window.open(url, '_blank', 'noopener,noreferrer');
  }

  if (url.startsWith("https") && !disable) {
    allow = true;
  }
</script>

<div class="flex justify-center items-center ">
<button id="uploadButton" class="  hover:bg-blue-700 font-bold py-4 px-8 rounded-lg mt-60 text-lg {toggle ? 'text-[#101820]' : 'text-[#FCF6F5]'}  bg-[#2BAE66]">
  Upload QR Code
</button>
<input type="file" id="fileInput" accept="image/*" style="display: none;">
</div>
<div class="font-bold py-4 px-8 rounded-lg text-lg {primarycol} {secondarycol} w-2/5 text-center mx-auto mt-32 border {toggle ? 'border-white' : 'border-slate-900 '} ">
<p>{qrCodeData}</p>
</div>

<div class="flex justify-center space-x-10 mt-20">
<button on:click={() => copyToClipboard(qrCodeData)} class=" hover:bg-blue-700  font-bold py-2 px-4 rounded {toggle ? 'text-[#101820]' : 'text-[#FCF6F5]'}  bg-[#2BAE66]" disabled="{disable}">
  Copy Text
</button>
<button on:click={navigateToNewPage} class=" hover:bg-blue-700 font-bold py-2 px-4 rounded {toggle ? 'text-[#101820]' : 'text-[#FCF6F5]'}  bg-[#2BAE66]" disabled="{!allow}">
  Go to URL Site
</button>
</div>
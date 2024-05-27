# ItemCollab

<div id="sprites"></div>
<script>
  async function fetchImages() {
    const response = await fetch('https://api.github.com/repos/MilesFarber/ItemCollab/contents/items');
    const data = await response.json();
    const pngFiles = data.filter(file => file.name.endsWith('.png'));
    const sprites = document.getElementById('sprites');
    pngFiles.forEach(file => {
      const img = new Image();
      img.onload = function() {
        console.log('Checking if image is 16x16');
        if(img.width === 16 && img.height === 16) {
          console.log(file.name + ' is 16x16');
          sprites.appendChild(img);
        } else {
          console.log(file.name + ' is not 16x16');
        }
      };
      img.src = file.download_url;
      img.alt = file.name;
    });
  }
  window.onload = fetchImages;
</script>
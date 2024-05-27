# ItemCollab

<div id="sprites"></div>
<script>
try {
async function fetchImages(path = 'https://api.github.com/repos/MilesFarber/ItemCollab/contents/items') {
  const response = await fetch(path);
  const data = await response.json();
  const sprites = document.getElementById('sprites');
  for (const file of data) {
    if (file.type === 'file' && file.name.endsWith('.png')) {
      const img = new Image();
      img.onload = function() {
        console.log('Checking if image is 16x16');
        if (img.width === 16 && img.height === 16) {
          console.log(file.name + ' is 16x16');
          sprites.appendChild(img);
        } else {
          console.log(file.name + ' is not 16x16');
        }
      };
      img.src = file.download_url;
      img.alt = file.name;
    } else if (file.type === 'dir') {
      await fetchImages(file.url);
    }
  }
}
window.onload = fetchImages;
} catch (error) {
  console.error('Caught error: ' + error.message);
}
</script>
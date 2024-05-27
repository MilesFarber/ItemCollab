# ItemCollab

<div id="sprites"></div>
<script>
try {
async function fetchImages(currentfolder = 'items') {
  const response = await fetch('https://api.github.com/repos/MilesFarber/ItemCollab/contents/' + currentfolder);
  const data = await response.json();
  const pngFiles = data.filter(file => file.name.endsWith('.png'));
  const sprites = document.getElementById('sprites');
  pngFiles.forEach(file => {
    const img = new Image();
    img.onload = function() {
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
} catch (error) {
  console.error('Caught error: ' + error.message);
}
</script>
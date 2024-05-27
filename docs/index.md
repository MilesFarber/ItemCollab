# ItemCollab

<div id="sprites"></div>
<script>
try {
  async function fetchImages(currentfolder = 'items') {
    console.log('https://api.github.com/repos/MilesFarber/ItemCollab/contents/' + currentfolder);
    const response = await fetch('https://api.github.com/repos/MilesFarber/ItemCollab/contents/' + currentfolder);
    const data = await response.json();
    const sprites = document.getElementById('sprites');
    for (const item of data) {
      if (item.type === 'file' && item.name.endsWith('.png')) {
        const img = new Image();
        img.onload = function() {
          console.log('Checking if image is 16x16');
          if (img.width === 16 && img.height === 16) {
            console.log(item.name + ' is 16x16');
            sprites.appendChild(img);
          } else {
            console.log(item.name + ' is not 16x16');
          }
        };
        img.src = item.download_url;
        img.alt = item.name;
      } else if (item.type === 'dir') {
        await fetchImages(currentfolder + '/' + item.name);
      }
    }
  }
  console.log('The fact that functions automatically use the windowonload event object as a parameter when the parameter is empty, WITHOUT the devs consent, is exactly why javascript should have never existed');
  window.onload = function() { fetchImages(); };
} catch (error) { console.error('Caught error: ' + error.message); }
</script>
# ItemCollab

<div id="sprites"></div>
<script>
try {
  async function fetchImages(currentfolder = 'items', parentElement = null) {
    if (!parentElement) { parentElement = document.getElementById('sprites'); }
    const folderDiv = document.createElement('div');
    folderDiv.id = currentfolder;
    parentElement.appendChild(folderDiv);
    console.log('https://api.github.com/repos/MilesFarber/ItemCollab/contents/' + currentfolder);
    const response = await fetch('https://api.github.com/repos/MilesFarber/ItemCollab/contents/' + currentfolder);
    const data = await response.json();
    for (const item of data) {
      if (item.type === 'file' && item.name.endsWith('.png')) {
        const img = new Image();
        img.onload = function() {
          if (img.width === 16 && img.height === 16) {
            folderDiv.appendChild(img);
          }
        };
        img.src = item.download_url;
        img.alt = item.name;
      } else if (item.type === 'dir') {
        await fetchImages(currentfolder + '/' + item.name, folderDiv);
      }
    }
  }
  console.log('The fact that functions automatically use the windowonload event object as a parameter when the parameter is empty, WITHOUT the devs consent, is exactly why javascript should have never existed');
  window.onload = function() { fetchImages(); };
} catch (error) { console.error('Caught error: ' + error.message); }
</script>
# ItemCollab

[Download ALL Item Sprites at once!](https://downgit.github.io/#/home?url=https://github.com/MilesFarber/ItemCollab/tree/trainer/items){: .btn }
[GitHub Repository + Placeholder License](https://github.com/MilesFarber/ItemCollab?tab=License-1-ov-file){: .btn }

<div id="sprites"></div>
<script>
try {
  async function fetchImages(currentfolder = 'items', parentElement = null) {
    if (!parentElement) { parentElement = document.getElementById('sprites'); }
    const folderDiv = document.createElement('div');
    folderDiv.id = currentfolder;
    parentElement.appendChild(folderDiv);
    const title = document.createElement('h3');
    title.textContent = currentfolder;
    folderDiv.appendChild(title);
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
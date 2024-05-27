# ItemCollab

<div id="sprites"></div>
<script>
window.onload = function() {
  fetchImages().catch(console.error);
};
async function fetchImages() {
  try {
    const response = await fetch('https://api.github.com/repos/MilesFarber/ItemCollab/contents/items');
    const data = await response.json();
    if (!response.ok) {
      throw new Error('Network response was not ok.');
    }
    processFolder(data).catch(console.error);
  } catch (error) {
    console.error('Fetch error: ' + error.message);
  }
}
async function processFolder(folder) {
  try {
    const folderResponse = await fetch(folder.url);
    const contentType = folderResponse.headers.get('content-type');
    if (!folderResponse.ok) {
      throw new Error('Network response was not ok.');
    }
    if (!contentType || !contentType.includes('application/json')) {
      throw new TypeError('Expected JSON response but received:', contentType);
    }
    const folderData = await folderResponse.json();
    for (const file of folderData) {
      if (file.type === 'dir') {
        await processFolder(file);
      } else if (file.name.endsWith('.png')) {
        const img = new Image();
        img.onload = function() {
          console.log('Checking if image is 16x16');
          if (img.width === 16 && img.height === 16) {
            console.log(file.name + ' is 16x16');
            document.getElementById('sprites').appendChild(img);
          } else {
            console.log(file.name + ' is not 16x16');
          }
        };
        img.onerror = function() {
          console.error('Error loading image: ' + file.name);
        };
        img.alt = file.name;
        img.src = file.download_url;
      }
    }
  } catch (error) {
    console.error('Process folder error: ' + error.message);
  }
}
</script>

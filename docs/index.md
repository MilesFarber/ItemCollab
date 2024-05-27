<div id="gallery"></div>
<script>
  async function fetchImages() {
    const response = await fetch('https://api.github.com/repos/username/ItemCollab/contents/items');
    const data = await response.json();
    const pngFiles = data.filter(file => file.name.endsWith('.png'));
    const gallery = document.getElementById('gallery');
    pngFiles.forEach(file => {
      const img = new Image();
      img.src = file.download_url; // Use the raw URL to display the image
      img.alt = file.name;
      gallery.appendChild(img);
    });
  }
  window.onload = fetchImages;
</script>

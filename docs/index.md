#ItemCollab

SpriteCollab, but for Item Sprites.

<div id="gallery"></div>

<script>
  <!-- This function fetches the list of files in the 'items' directory -->
  async function fetchImages() {
    const response = await fetch('https://api.github.com/repos/MilesFarber/ItemCollab/items');
    const data = await response.json();
    
    <!-- If image is PNG -->
    const pngFiles = data.filter(file => file.name.endsWith('.png'));
    
    <!-- Create image elements and append to gallery -->
    const gallery = document.getElementById('gallery');
    pngFiles.forEach(file => {
      const img = new Image();
      img.src = file.download_url; 
      <!-- Use raw url -->
      img.alt = file.name;
      gallery.appendChild(img);
    });
  }

  <!-- Call function when window loads -->
  window.onload = fetchImages;
</script>

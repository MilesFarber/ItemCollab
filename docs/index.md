#ItemCollab

SpriteCollab, but for Item Sprites.

<div id="gallery"></div>

<script>
  // This function fetches the list of files in the 'items' directory
  async function fetchImages() {
    const response = await fetch('https://api.github.com/repos/MilesFarber/ItemCollab/contents/items');
    const data = await response.json();
    
    // Filter out only PNG files
    const pngFiles = data.filter(file => file.name.endsWith('.png'));
    
    // Create image elements and append them to the gallery
    const gallery = document.getElementById('gallery');
    pngFiles.forEach(file => {
      const img = new Image();
      img.src = file.download_url; // Use the raw URL to display the image
      img.alt = file.name;
      gallery.appendChild(img);
    });
  }

  // Call the function when the window loads
  window.onload = fetchImages;
</script>

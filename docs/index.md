# ItemCollab

<div id="sprites"></div> <!--Declare the container for all sprites. Remember, all comments are // inside scripts.-->
<script> //Start javascript
  async function fetchimages(path = 'https://api.github.com/repos/MilesFarber/ItemCollab/contents/items') { //Create a fetchimages function and set the image folder as default path
    const response = await fetch(path); //Fetch the response path of every file inside the default path and write it as a constant
    const data = await response.json(); //Get the json data of all files in the default path and write it as a constant
    for (const file of data) { //For each file
      if (file.type === 'file' && file.name.endsWith('.png')) { //If the file is a PNG
        const img = new Image(); //Set the PNG image as a constant
        img.onload = function() { //When loading the image
          console.log('Checking if image is 16x16'); //Debug
          if (img.width === 16 && img.height === 16) { //If the image is 16x16
            console.log(file.name + ' is 16x16'); //Debug
            document.getElementById('sprites').appendChild(img); //Add the image to the sprites container
          } else { //If the image is not 16x16
            console.log(file.name + ' is not 16x16'); //Debug
          } //End if 16x16
        }; //End image load
        img.src = file.download_url; //Get the raw url
        img.alt = file.name; //Why is this even required wtf javascript
      } else if (file.type === 'dir') { // But if it's a folder
        await fetchimages(file.url); // Perform fetchimages but for each subfolder
      } //End if PNG
    } //End for each file
  }//End fetchimages function
  window.onload = fetchimages; //Run fetchimages function when site is loaded
</script> <!--Close javascript-->
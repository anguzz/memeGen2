<script>
    let img, fileinput;
    let temp = "/temp.png";
    let uploadImg = "/upload.png";
    let topText = "";
    let bottomText = "";
    let textColor = "#ffffff";
    
    const onFileSelected = (e) => {
      let image = e.target.files[0];
      let reader = new FileReader();
      reader.readAsDataURL(image);
      reader.onload = (e) => {
        img = e.target.result;
      };
    };
  
    const saveImage = () => {
      const canvas = document.createElement("canvas");
      const ctx = canvas.getContext("2d");
      const image = new Image();
      image.src = img;
      image.onload = () => {
        canvas.width = image.width;
        
        canvas.height = image.height;
        ctx.drawImage(image, 0, 0);
        ctx.fillStyle = textColor;
        ctx.font = "bold 4vw Arial";
        ctx.textAlign = "center";
        ctx.fillText(topText, canvas.width / 2, 50);
        ctx.fillText(bottomText, canvas.width / 2, canvas.height - 50);
        const link = document.createElement("a");
        link.download = "image.png";
        link.href = canvas.toDataURL();
        link.click();
      };
    };
    
    const changeTextColor = () => {
      const colorPicker = document.getElementById("color-picker");
      textColor = colorPicker.value;
    };
  </script>
  
  <div id="app">
    {#if img}
      <img class="tempImg" src="{img}" alt="d" />
    {:else}

    {/if}

    <div class="upload-wrapper">
        <img class="upload" src={uploadImg} alt="" on:click={() => { fileinput.click(); }} />
        <div class="chan" on:click={() => { fileinput.click(); }}>Upload Image</div>
        <input style="display:none" type="file" accept=".jpg, .jpeg, .png" on:change={(e) => onFileSelected(e)} bind:this={fileinput} />
      </div>

      <br><br>
    <div class="caption-form">
      <label for="top-text">Top Caption:</label>
      <input type="text" id="top-text" bind:value={topText} />
  
      <label for="bottom-text">Bottom Caption:</label>
      <input type="text" id="bottom-text" bind:value={bottomText} />
  
      <label for="color-picker">Text Color:</label>
      <input type="color" id="color-picker" value={textColor} on:change={changeTextColor} />
      <br>
      
      <button class="button" on:click={saveImage}>Save Image</button>
    </div>
  
   
  </div>
  
    
<style>
	#app{
	display:flex;
		align-items:center;
		justify-content:center;
		flex-flow:column;
        padding:20px;
}
input[type="text"], textarea {

background-color : #2b2929; 

}
	.upload{
		display:flex;
	  height:50px;
		width:50px;
		cursor:pointer;
    margin-left:20px;
	}
	.tempImg{
		display:flex;

	}


    .button{
        background-color: rgb(53, 73, 223);
        border-radius: 10px;
        margin:10px;
        padding:10px;
    }
    button:hover
    {
        background-color: rgb(141, 53, 223);
    }
</style>
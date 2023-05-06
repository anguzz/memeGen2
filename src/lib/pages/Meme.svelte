<script>
    import { saveAs } from 'file-saver';
    
    let img, fileinput, caption = "";
    const temp = "/temp.png";
    const uploadImg = "/upload.png";
    
    const onFileSelected = (e) => {
      const image = e.target.files[0];
      const reader = new FileReader();
      reader.readAsDataURL(image);
      reader.onload = e => {
        img = e.target.result;
      };
    };
    
    const saveImage = () => {
      const canvas = document.createElement("canvas");
      const context = canvas.getContext("2d");
      const image = new Image();
      image.src = img;
      image.onload = () => {
        canvas.width = image.width;
        canvas.height = image.height;
        context.drawImage(image, 0, 0);
        context.fillStyle = "white";
        context.font = "bold 20px Arial";
        context.textAlign = "center";
        context.fillText(caption, canvas.width / 2, canvas.height - 30);
        canvas.toBlob(blob => {
          saveAs(blob, "image-with-caption.png");
        });
      };
    };
  </script>
  
  <div id="app">
    {#if img}
      <div style="position: relative;">
        <img class="tempImg" src="{img}" alt="d" />
        <div class="caption-prev">
          {caption}
        </div>
      </div>
    {:else}
    {/if}
  
    <br><br>
    <img class="upload" src={uploadImg} alt="" on:click={() => { fileinput.click(); }} />
    <div class="chan" on:click={() => { fileinput.click(); }}>Upload Image</div> 
    <br>
  
    <input style="display:none" type="file" accept=".jpg, .jpeg, .png" on:change={(e) => onFileSelected(e)} bind:this={fileinput} />
    <input type="text" placeholder="Enter caption" bind:value={caption} color="black"/>
    <button class="button" on:click={saveImage}>Save Image</button>
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
	}
	.tempImg{
		display:flex;

	}

    .caption-prev{
        position: absolute;
        bottom: 0;
        left: 0; 
        right: 0;
        padding: 5px;
        text-align: center;
        color:rgb(251, 0, 255);
        font-weight: 400;
        font-size:x-large;
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
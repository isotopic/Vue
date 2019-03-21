<template>
	<div id="html-zip-container">

		<label>
			<span class="form-label">Selecione o arquivo .zip:  </span>
			<input type="file" accept="application/zip" id="file-input">
		</label>

		<div id="file-list" v-if="fileList.length">
			<div class="file-list-item" v-for="item in fileList" @click="getContent(item)">{{ item.name }}</div>
		</div>

		<div  v-if="fileTitle">
			<h4 align='right'>{{fileTitle}}</h4>
			<div id="file-content">{{fileContent}}</div>		
			<img id="file-image"></img>
		</div>

	</div>
</template>

<script>

import JSZip from 'jszip'

export default {

	name: 'Html5Preview',

	props: {
		msg: String
	},

	data: function () {
		return {
			fileInput: '',
			fileList: [], 
			fileTitle: '',
			fileContent: '',
			imgSrc: ''
		}
	},

	methods:{

		init: function(){
			this.fileInput = document.querySelector('#file-input');
			this.fileInput.addEventListener('change', this.onFileSelect);
		},

		onFileSelect: function(e){
			this.updateList(this.fileInput.files[0]);
		},

		updateList: function(f){
			this.fileList = [];
        	JSZip.loadAsync(f).then((zip)=> {
        		zip.forEach((path, item) => {
    				this.fileList.push(item);
				})
        	}), (error)=>{
        		console.log('erro no zip');
        	}
        },

        getContent: function(f){
        	this.fileTitle = f.name;
        	this.fileContent = '';

        	if(this.fileIsImage(f)){
				f.async("arraybuffer").then(data => {
					var buffer = new Uint8Array(data);
					var blob = new Blob([buffer.buffer]);
					var img = document.querySelector("#file-image");
					img.src = URL.createObjectURL(blob);
				});
        	}else{
        		f.async('text').then(data => {
        			document.querySelector("#file-image").src='';
					this.fileContent = data;
				});
        	}
        },

        fileIsImage: function(f){
			return (/\.(gif|jpg|jpeg|png)$/i).test(f.name);
        }

    },

	mounted: function(){
		this.init();
	}
}


</script>


<style scoped>

#html-zip-container{
	text-align: left;
	border: 1px solid #ccc;
	max-width: 600px;
	margin: 0 auto;
	padding: 40px;
	border-radius: 5px;
}

#file-list{
	margin-top: 20px;
}

.file-list-item{
	cursor: pointer;
	color: #999;
	border-top: 1px dotted #ccc;
	text-align: left;
	padding: 7px 5px;
}

.file-list-item:hover{
	color:#222;
}

#file-content{
	padding: 10px;
	font-size: 12px;
	background-color: #f0f0f0;
	margin-top: 20px;
	border-radius: 3px;
}

#file-image{
	width:100%;
}
</style>

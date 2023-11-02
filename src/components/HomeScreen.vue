<!--
MedScript Lite
Copyright (C) 2023 Dr. Agnibho Mondal
This file is part of MedScript Lite.
MedScript Lite is free software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.
MedScript Lite is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License for more details.
You should have received a copy of the GNU General Public License along with MedScript Lite. If not, see <https://www.gnu.org/licenses/>.
-->

<template>
	<div id="home-screen">
		<form class="mb-4">
			<div class="input-group">
				<span class="input-group-text">Select File</span>
				<input type="file" accept=".mpaz" class="form-control" v-on:change="loadFile">
				<button type="submit" class="btn btn-primary">Open</button>
			</div>
		</form>
		<div class="row">
			<div class="col-sm-4 d-grid gap-2">
				<button class="btn btn-success" @click="newFile">New File</button>
			</div>
			<div class="col-sm-4 d-grid gap-2">
				<button class="btn btn-primary" @click="prescriber">Prescriber</button>
			</div>
			<div class="col-sm-4 d-grid gap-2">
				<button class="btn btn-info" @click="about">About</button>
			</div>
		</div>
		<div v-show="showAbout" class="alert alert-info mt-4">
			<p>MedScript Lite version 0.1</p>
			<p>MedScript Lite is a JavaScript implementation of MedScript. It has a limited feature set but it can run on any device with a modern web browser.</p>
			<p><a href="https://code.agnibho.com/medscript">https://code.agnibho.com/medscript</a></p>
			<p>Copyright &copy; 2023{{new Date().getFullYear()>2023 ? "-"+new Date().getFullYear() : ""}} Dr. Agnibho Mondal</p>
		</div>
	</div>
</template>

<script>
	import JSZip from "jszip"

	export default {
		data() {
			return {
				filename: "",
				openedPrescription: Object,
				openedAttachment: [],
				showAbout: false
			}
		},
		methods: {
			loadFile(e) {
				var filename=e.target.files[0];
				if (filename) {
					var reader=new FileReader();
					reader.readAsArrayBuffer(filename);
					reader.onload=function(e) {
						var zipdata=e.target.result;
						var zip=new JSZip();
						zip.loadAsync(zipdata)
							.then(function(zip) {
								this.openedAttachment=[]
								var entries=Object.entries(zip.files)
								entries=entries.map((item)=>item[0])
								var attachName=entries.filter((element)=>element.startsWith("attachment/") && element!="attachment/")
								if(attachName.length==0) {
									this.$emit("openAttachment", this.openedAttachment);
								}
								var attachFile=[]
								attachName.forEach((item)=>attachFile.push(zip.files[item]))
								attachFile.forEach(function(item, index) {
									item.async("arraybuffer")
										.then(function(content) {
											this.openedAttachment.push({"name": attachName[index].replace("attachment/", ""), "content": content});
											if(index==attachName.length-1){
												this.$emit("openAttachment", this.openedAttachment);
											}
										}.bind(this));
								}.bind(this));
								var presfile=zip.files["prescription.json"];
								presfile.async("string")
									.then(function(content) {
										this.openedPrescription=JSON.parse(content);
										this.$emit("openPrescription", this.openedPrescription);
									}.bind(this));
							}.bind(this));
					}.bind(this)
				}
			},
			newFile() {
				this.$emit("newFile")
			},
			prescriber() {
				this.$emit("editPrescriber")
			},
			about() {
				this.showAbout=!this.showAbout
			}
		}
	}
</script>

<style>
#home-screen {
	text-align: center;
}
</style>

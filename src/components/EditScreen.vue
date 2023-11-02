<!--
MedScript Lite
Copyright (C) 2023 Dr. Agnibho Mondal
This file is part of MedScript Lite.
MedScript Lite is free software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.
MedScript Lite is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License for more details.
You should have received a copy of the GNU General Public License along with MedScript Lite. If not, see <https://www.gnu.org/licenses/>.
-->

<template>
	<div id="edit-screen">
		<form @submit.prevent="saveData">
			<div class="form-group row mb-2">
				<label for="date" class="col-sm-2">Date:</label>
				<div class="col-sm-10">
					<input type="datetime-local" step="any" class="form-control" id="date" v-model="prescription.date">
				</div>
			</div>
			<div class="form-group row mb-2">
				<label for="id" class="col-sm-2">ID:</label>
				<div class="col-sm-10">
					<input type="text" class="form-control" id="id" v-model="prescription.id">
				</div>
			</div>
			<div class="form-group row mb-2">
				<label for="name" class="col-sm-2">Name:</label>
				<div class="col-sm-10">
					<input type="text" class="form-control" id="name" v-model="prescription.name">
				</div>
			</div>
			<div class="form-group row mb-2">
				<label for="age" class="col-sm-2">Age:</label>
				<div class="col-sm-10">
					<input type="text" class="form-control" id="age" v-model="prescription.age">
				</div>
			</div>
			<div class="form-group row mb-2">
				<label for="sex" class="col-sm-2">Sex:</label>
				<div class="col-sm-10">
					<input type="text" class="form-control" id="sex" v-model="prescription.sex">
				</div>
			</div>
			<div class="form-group row mb-2">
				<label for="address" class="col-sm-2">Address:</label>
				<div class="col-sm-10">
					<input type="text" class="form-control" id="address" v-model="prescription.address">
				</div>
			</div>
			<div class="form-group row mb-2">
				<label for="contact" class="col-sm-2">Contact:</label>
				<div class="col-sm-10">
					<input type="text" class="form-control" id="contact" v-model="prescription.contact">
				</div>
			</div>
			<div class="form-group row mb-2">
				<label for="diagnosis" class="col-sm-2">Diagnosis:</label>
				<div class="col-sm-10">
					<input type="text" class="form-control" id="diagnosis" v-model="prescription.diagnosis">
				</div>
			</div>
			<div class="form-group row mb-2">
				<label for="extra" class="col-sm-2">Extra/Certificate:</label>
				<div class="col-sm-10">
					<textarea class="form-control" id="extra" v-model="prescription.extra"></textarea>
				</div>
			</div>
			<div class="form-group row mb-2">
				<label for="mode" class="col-sm-2">Mode:</label>
				<div class="col-sm-10">
					<input type="text" class="form-control" id="mode" v-model="prescription.mode">
				</div>
			</div>
			<div class="form-group row mb-2">
				<div class="col-sm-2">DAW:</div>
				<div class="col-sm-10">
					<div class="form-check">
						<input type="checkbox" class="form-check-input" id="daw" v-model="prescription.daw">
						<label class="form-check-label" for="daw">Dispense as written</label>
					</div>
				</div>
			</div>
			<hr>
			<div class="form-group row">
				<label for="note" class="col-sm-2">Clinical Notes:</label>
				<div class="col-sm-10">
					<textarea class="form-control" id="note" v-model="prescription.note"></textarea>
				</div>
			</div>
			<hr>
			<div class="form-group row">
				<label for="report" class="col-sm-2">Reports:</label>
				<div class="col-sm-10">
					<textarea class="form-control" id="report" v-model="prescription.report"></textarea>
				</div>
			</div>
			<hr>
			<div class="form-group row">
				<label for="advice" class="col-sm-2">Advice:</label>
				<div class="col-sm-10">
					<textarea class="form-control" id="advice" v-model="prescription.advice"></textarea>
				</div>
			</div>
			<hr>
			<div class="form-group row">
				<label for="investigation" class="col-sm-2">Investigations:</label>
				<div class="col-sm-10">
					<textarea class="form-control" id="investigation" v-model="prescription.investigation"></textarea>
				</div>
			</div>
			<hr>
			<div class="form-group row">
				<label for="medication" class="col-sm-2">Medications:</label>
				<div class="col-sm-10">
					<textarea class="form-control" id="medication" v-model="prescription.medication"></textarea>
				</div>
			</div>
			<hr>
			<div class="form-group row">
				<label for="additional" class="col-sm-2">Additional:</label>
				<div class="col-sm-10">
					<textarea class="form-control" id="additional" v-model="prescription.additional"></textarea>
				</div>
			</div>
			<hr>
			<div class="form-group row mb-2">
				<label for="attachment" class="col-sm-2">Attachments:</label>
				<div class="col-sm-10">
					<ul class="list-group" id="attachment">
						<li class="list-group-item" v-for="(item, index) in attachment" :key="index" @click="selectAttachment(index)" :class="isSelected(index)">
							{{item.name}}
						</li>
					</ul>
				</div>
			</div>
			<div class="form-group row">
				<div class="col-sm-6 offset-sm-2">
					<input type="file" class="form-control" @change="loadAttachment($event)">
				</div>
				<div class="col-sm-2 d-grid gap-2">
					<button type="button" class="btn btn-secondary" @click="downloadAttachment">Download</button>
				</div>
				<div class="col-sm-2 d-grid gap-2">
					<button type="button" class="btn btn-secondary" @click="removeAttachment">Remove</button>
				</div>
			</div>
			<hr>
			<div class="form-group row mb-2">
				<label for="template" class="col-sm-2">Template</label>
				<div class="col-sm-10">
					<select class="form-select mb-2" v-model="template">
						<option value=default>Default Template</option>
						<option value="medcert">Medical Certificate</option>
					</select>
				</div>
			</div>
			<div class="form-group row">
				<div class="col-sm-3 offset-sm-3 d-grid gap-2">
					<button type="submit" class="btn btn-primary">Save</button>
				</div>
				<div class="col-sm-3 d-grid gap-2">
					<button type="button" @click="renderPrescription" class="btn btn-success">Render</button>
				</div>
				<div class="col-sm-3 d-grid gap-2">
					<button type="button" @click="cancel" class="btn btn-warning">Cancel</button>
				</div>
			</div>
		</form>
	</div>
</template>
<script>
	import JSZip from "jszip"
	export default {
		props: ["openedPrescription", "openedAttachment"],
		data() {
			return {
				prescription: this.newData(),
				attachment: [],
				selected: 0,
				template: "default"
			}
		},
		methods: {
			newData() {
				var prescription={
					date: new Date(new Date() - new Date().getTimezoneOffset() * 60000).toISOString().split(".")[0],
					id: "",
					name: "",
					age: "",
					sex: "",
					address: "",
					contact: "",
					diagnosis: "",
					extra: "",
					daw: "",
					mode: "",
					note: "",
					report: "",
					advice: "",
					investigation: "",
					medication: "",
					additional: "",
					attachment: "",
					prescriber: this.loadPrescriber()
				};
				return prescription;
			},
			saveData() {
				var prescription=JSON.stringify(this.prescription, null, 2);
				var filename=""
				if(this.prescription.id){
					filename=this.prescription.id.name.replace(/ +/g, "_")
				}
				else if(this.prescription.name){
					filename=this.prescription.name.replace(/ +/g, "_")
				}
				else {
					filename="prescription"
				}
				var zipfile=new JSZip();
				zipfile.file("prescription.json", prescription);
				zipfile.file("meta.json", JSON.stringify({"type": "MedScript", "version": "0.2"}))
				this.attachment.forEach((item) => {
					zipfile.file("attachment/"+item.name, item.content)
				});
				zipfile.generateAsync({ type: "blob" })
					.then(function (blob) {
						var link=document.createElement("a");
						link.href=URL.createObjectURL(blob);
						link.download=filename+".mpaz";
						link.click();
					});
			},
			loadPrescriber() {
				return JSON.parse(localStorage.getItem("prescriber"))
			},
			renderPrescription() {
				this.$emit("renderPrescription", this.prescription, this.template);
			},
			cancel() {
				this.$emit("cancel");
			},
			selectAttachment(index) {
				this.selected=index
			},
			isSelected(index) {
				if(index==this.selected) {
					return("active");
				}
				else {
					return("");
				}
			},
			loadAttachment(event){
				var filename=event.target.files[0]
				var reader=new FileReader();
				reader.readAsArrayBuffer(filename);
				reader.onload=function(e) {
					this.attachment.push({"name": event.target.files[0]["name"], "content": e.target.result});
				}.bind(this)
			},
			removeAttachment(){
				this.attachment.splice(this.selected, 1);
				this.selected=0;
			},
			downloadAttachment(){
				try {
					var link=document.createElement("a");
					link.href=URL.createObjectURL(new Blob([this.attachment[this.selected].content]));
					link.download=this.attachment[this.selected].name;
					link.click();
				}
				catch (e) {
					console.log(e.message)
				}
			}
		},
		watch: {
			openedPrescription: function(){
				if(this.openedPrescription) {
					this.prescription=structuredClone(this.openedPrescription)
				}
				else {
					this.prescription=this.newData()
				}
			},
			openedAttachment: function(){
				this.attachment=structuredClone(this.openedAttachment)
			}
		}
	};
</script>

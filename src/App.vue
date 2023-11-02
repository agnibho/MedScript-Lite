<!--
MedScript Lite
Copyright (C) 2023 Dr. Agnibho Mondal
This file is part of MedScript Lite.
MedScript Lite is free software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.
MedScript Lite is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License for more details.
You should have received a copy of the GNU General Public License along with MedScript Lite. If not, see <https://www.gnu.org/licenses/>.
-->

<template>
	<div id="app">
		<div class="container-fluid">
			<div v-show="!showComponent.render">
				<div class="alert alert-primary text-center">
					<h1>MedScript Lite</h1>
				</div>
				<hr>
			</div>
			<home-screen @openPrescription="loadPrescription" @openAttachment="loadAttachment" @newFile="newFile" @editPrescriber="editPrescriber" v-show="showComponent.home"></home-screen><br>
			<edit-screen @renderPrescription="renderPrescription" @cancel="cancelEdit" :openedPrescription="openedPrescription" :openedAttachment="openedAttachment" v-show="showComponent.edit"></edit-screen>
			<render-screen :prescription="prescription" :template="template" @closeRender="closeRender" v-show="showComponent.render"></render-screen>
			<prescriber-screen @cancel="cancelPrescriber" v-show="showComponent.prescriber"></prescriber-screen>
			<div v-show="!showComponent.render">
				<hr>
			</div>
		</div>
	</div>
</template>

<script>
	import "bootstrap/dist/css/bootstrap.min.css"
	import "bootstrap"
	import HomeScreen from "./components/HomeScreen.vue"
	import EditScreen from "./components/EditScreen.vue"
	import RenderScreen from "./components/RenderScreen.vue"
	import PrescriberScreen from "./components/PrescriberScreen.vue"

	export default {
		name: "MedScript Lite",
		data() {
			return {
				openedPrescription: {
					date: "",
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
					attachment: ""
				},
				openedAttachment: [],
				prescription: null,
				template: null,
				showComponent: {home: true, edit: false, render: false, prescriber: false}
			}
		},
		methods: {
			loadPrescription(data) {
				this.openedPrescription=data;
				this.showComponent.home=false;
				this.showComponent.edit=true;
			},
			loadAttachment(data) {
				this.openedAttachment=data;
			},
			renderPrescription(prescription, template) {
				this.prescription=structuredClone(prescription);
				this.template=structuredClone(template);
				this.showComponent.edit=false;
				this.showComponent.render=true;
			},
			closeRender() {
				console.log("close")
				this.showComponent.render=false;
				this.showComponent.edit=true;
			},
			cancelEdit() {
				this.showComponent.edit=false;
				this.showComponent.home=true;
			},
			editPrescriber() {
				this.showComponent.home=false;
				this.showComponent.prescriber=true;
			},
			cancelPrescriber() {
				this.showComponent.prescriber=false;
				this.showComponent.home=true;
			},
			newFile() {
				this.openedPrescription=null;
				this.showComponent.home=false;
				this.showComponent.edit=true;
			},
		},
		components: {
			HomeScreen,
			EditScreen,
			RenderScreen,
			PrescriberScreen
		}
	}
</script>

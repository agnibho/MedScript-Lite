<!--
MedScript Lite
Copyright (C) 2023 Dr. Agnibho Mondal
This file is part of MedScript Lite.
MedScript Lite is free software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.
MedScript Lite is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License for more details.
You should have received a copy of the GNU General Public License along with MedScript Lite. If not, see <https://www.gnu.org/licenses/>.
-->

<template>
	<div id="prescriber-screen">
		<form @submit.prevent="save">
			<div class="form-group row mb-2">
				<label for="name" class="col-sm-2">Name:</label>
				<div class="col-sm-10">
					<input type="text" class="form-control" id="name" v-model="prescriber.name">
				</div>
			</div>
			<div class="form-group row mb-2">
				<label for="qualification" class="col-sm-2">Qualification:</label>
				<div class="col-sm-10">
					<input type="text" class="form-control" id="qualification" v-model="prescriber.qualification">
				</div>
			</div>
			<div class="form-group row mb-2">
				<label for="registration" class="col-sm-2">Registration:</label>
				<div class="col-sm-10">
					<input type="text" class="form-control" id="registration" v-model="prescriber.registration">
				</div>
			</div>
			<div class="form-group row mb-2">
				<label for="address" class="col-sm-2">Address:</label>
				<div class="col-sm-10">
					<textarea class="form-control" id="address" v-model="prescriber.address"></textarea>
				</div>
			</div>
			<div class="form-group row mb-2">
				<label for="contact" class="col-sm-2">Contact:</label>
				<div class="col-sm-10">
					<textarea class="form-control" id="contact" v-model="prescriber.contact"></textarea>
				</div>
			</div>
			<div class="form-group row mb-2">
				<label for="extra" class="col-sm-2">Extra:</label>
				<div class="col-sm-10">
					<textarea class="form-control" id="extra" v-model="prescriber.extra"></textarea>
				</div>
			</div>
			<div class="form-group row">
				<div class="col-sm-3 offset-sm-3 d-grid gap-2">
					<button type="submit" class="btn btn-primary">Save</button>
				</div>
				<div class="col-sm-3 d-grid gap-2">
					<button type="button" @click="cancel" class="btn btn-warning">Cancel</button>
				</div>
				<div class="col-sm-3 d-grid gap-2">
					<button type="button" @click="reset" class="btn btn-danger">Reset</button>
				</div>
			</div>
		</form>
	</div>
</template>

<script>
	export default {
		data() {
			return {
				prescriber: this.loadPrescriber()
			}
		},
		methods: {
			loadPrescriber() {
				var prescriber;
				try {
					prescriber=JSON.parse(localStorage.getItem("prescriber"));
				}
				catch {
					prescriber={name: "", qualification: "", registration: "", address: "", contact: "", extra: "", properties: null}
				}
				return prescriber;
			},
			save() {
				localStorage.setItem("prescriber", JSON.stringify(this.prescriber));
				this.$emit("save");
			},
			cancel() {
				this.$emit("cancel");
			},
			reset() {
				this.prescriber={name: "", qualification: "", registration: "", address: "", contact: "", extra: "", properties: null}
			}
		}
	}
</script>

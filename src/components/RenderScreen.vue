<!--
MedScript Lite
Copyright (C) 2023 Dr. Agnibho Mondal
This file is part of MedScript Lite.
MedScript Lite is free software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.
MedScript Lite is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License for more details.
You should have received a copy of the GNU General Public License along with MedScript Lite. If not, see <https://www.gnu.org/licenses/>.
-->

<template>
	<div id="render-screen">
		<div v-html="display"></div>
		<button @click="closeRender" class="btn btn-warning" style="position: absolute; right: 0px; top: 0px">Close</button>
	</div>
</template>
<script>
	import nunjucks from "nunjucks"
	import dateFilter from "nunjucks-date-filter"
	export default {
		props: ["prescription", "template"],
		data() {
			return {
				display: ""
			}
		},
		methods: {
			render() {
				fetch("template/"+this.template+".html")
					.then((response)=>response.text())
					.then(function(response) {
						nunjucks.configure({autoescape: true}).addFilter("date", dateFilter);
						var rendered=nunjucks.renderString(response, this.prescription);
						rendered=new DOMParser().parseFromString(rendered, "text/html")
						this.display=rendered.body.innerHTML
					}.bind(this));
			},
			closeRender() {
				this.$emit("closeRender");
			}
		},
		watch: {
			prescription: function() {
				this.render();
			}
		}
	};
</script>

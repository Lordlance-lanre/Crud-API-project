<template>
	<div class="home bg-indigo-300 sm:w-full max-w-full h-screen">
		<div class="container">
			<h1 class="p-10 text-white text-2xl font-medium ">Crud App page</h1>
		</div>

		<div class="mx-12 mt-10 lg:mx-6">
			<div class="mb-6">
				<label class="block mb-2 text-sm font-medium text-gray-900">Name</label>
				<input type="text" class="block mb-2 text-sm font-medium text-gray-900 lg:w-6/12 rounded p-2 outline-none" v-model="name" id="same">
			</div>
			<div class="mb-6">
				<label class="block mb-2 text-sm font-medium text-gray-900 ">Rank</label>
				<input type="text" class="lg:w-6/12 block mb-2 rounded p-2 text-sm font-medium outline-none text-gray-900" v-model="rank" id="same">
			</div>
			<div class="mb-6">
				<label class="block mb-2 text-sm font-medium text-gray-900 ">Activity</label>
				<input type="checkbox" class="block mb-2 text-sm font-medium text-gray-900"  id="same">
			</div>
			<div class="">
				<label class="block mb-2 text-sm font-medium text-gray-900 ">Description</label>
				<textarea type="text" class="lg:w-6/12 block mb-2 text-sm font-medium rounded p-2 text-gray-900 outline-none" v-model="description" id="same"></textarea>
			</div>
			<button @click="newInput" class="mt-3 bg-black text-white p-2 rounded lg:px-10 ">
				submit
			</button>
		</div>

		<div class="mt-20 bg-indigo-300">
			<table>
			  <tr class="text-sm lg:text-lg">
			    <th>id</th>
			    <th>name</th>
			    <th>rank</th>
			    <th>activity</th>
			    <th>description</th>
			    <th></th>
			  </tr>
			  <tr v-for="item in database" :key="item.id" class="text-sm lg:text-lg">
			    <td>{{item.id}}</td>
			    <td>{{item.name}}</td>
			    <td>{{item.rank}}</td>
			    <td>{{item.isEmployed}}</td>
			    <td>{{item.description}}</td>
			    <td><button class="bg-black text-white p-1 lg:p-2 rounded"
			      @click="deleteData(item)">Delete</button></td>
			  </tr>
			</table>
	 	</div>
	</div>
</template>


<script setup>
import { ref, onMounted } from "vue"
import axios from "axios"

const name = ref("")
const rank = ref(" ")
const active = ref(" ")
const description = ref("")
const database = ref([])
const baseUrl = ref("http://localhost:7000/database")

/** --- function to get request of resources --- **/
const newData = () => {
	axios.get(baseUrl.value)
		.then((res) => {database.value = res.data; console.log(res.data)})
}

onMounted(() => {
	newData()
})

/** --- function to delete each row of item --- **/
const deleteData = (item) => {
	database.value.forEach((row, rowId) => {
		if(item == row){
			database.value.splice(rowId, 1)
		}
		console.log("deleted ->", item.id);
	})
}

/** --- function to post new row of item --- **/
const newInput = () => {
	let inputValues = axios.post(baseUrl.value, {
		name: name.value,
		rank: rank.value,
		isEmployed: active.value,
		description: description.value
	});

	/**const stringValue = JSON.stringify(inputValues);**/


	location.reload(inputValues);
	database.value.push(inputValues);
	console.log("form input >>", database.value);
} 

/**const checkIfClicked = ($event) =>{
	if($event.isTrusted === true){
		inputValues.active == "active";
	}else{
		inputValues.active == "false";
	}
	console.log('event', $event)
}


**/
</script>


<style scoped>
table {
  font-family: arial, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

td, th {
  border: 1px solid #dddddd;
  text-align: left;
  padding: 8px;
}

tr:nth-child(even) {
  background-color: #dddddd;
}

@media only screen and (max-width: 500px) {
  .home{
  	width: 100%;
  }

}	
</style>
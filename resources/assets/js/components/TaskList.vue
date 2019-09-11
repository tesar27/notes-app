<template>
   <div class='row'>
		
      <h1>My Notes</h1>
      <h4>New Note</h4>
      <form action="#" @submit.prevent="createTask()">
         <div class="input-group">
            <input v-model="task.title" type="text" name="title" class="form-control" autofocus>
            <span class="input-group-btn">
            <button type="submit" class="btn btn-primary">New Note</button>
            </span>
         </div>
         <div class="form-group">
            <label for="exampleFormControlTextarea1">Description</label>
            <textarea class="form-control" v-model="task.body" name="body" id="exampleFormControlTextarea1" rows="3"></textarea>
         </div>
      </form>
      <h4>All Notes</h4>
      <div class="row text-center" v-if="loading">
         <rotate-square2></rotate-square2>
      </div>
      <ul class="list-group" v-if="!loading">
         <li v-if='list.length === 0'>There are no tasks yet!</li>
         <li class="list-group-item" v-for="task in list" v-bind:key="task.index">
            <div class="container">
               <div class="row" >
                  <div class="col-xs-4 col-sm-4 col-md-4 col-lg-4">
                     {{task.title}}
                  </div>
                  <div class="col-xs-4 col-sm-4 col-md-4 col-lg-4">
                     {{ task.body }}
                  </div>
                  <div class="col-xs col-sm col-md col-lg-12"> <button @click="deleteTask(task.id)" class="btn btn-danger btn-xs pull-right">Delete</button>
                  </div>
               </div>
            </div>
         </li>
      </ul>
</div>
	 
</template>
<script>
import { RotateSquare2 } from "vue-loading-spinner";
export default {
	components: {
		RotateSquare2
	},
	data() {
		return {
			loading: true,
			list: [],
			task: {
				id: "",
				title: "",
				body: ""
			}
		};
	},

	created() {
		this.fetchTaskList();
	},

	methods: {
		fetchTaskList() {
			axios
				.get("api/tasks")
				.then(res => {
					this.list = res.data;
					this.loading = false;
				})
				.catch(err => console.error(err));
		},

		createTask() {
			axios
				.post("api/tasks", this.task)
				.then(res => {
					this.fetchTaskList();
					this.task.title = "";
					this.task.body = "";
				})
				.catch(err => console.error(err));
		},

		setVal(id, title, body) {
			this.id = id;
			this.title = title;
			this.body = body;
		},

		editItem() {
			var t_val = document.getElementById("e_title");
			var b_val = document.getElementById("e_body");
			axios
				.post(api / tasks, (this.task.title = t_val), (this.task.body = b_val))
				.then(res => {});
		},

		deleteTask(id) {
			axios
				.delete("api/tasks/" + id)
				.then(res => {
					this.fetchTaskList();
				})
				.catch(err => console.error(err));
		}
	}
};
</script>
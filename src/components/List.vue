<template>
	<div>
		<ul class="list-group mt-2">
			<li v-for="(todo,index) in todos" :key="index" class="list-group-item">
				<div class="row">
					<div class="col-10 text-">
						<span v-if="todo.isDone">
							<del>{{ todo.act }}</del>
						</span>
						<span v-else>
							{{ todo.act }}
						</span>
					</div>
					<div class="col-1">
						<button type="button" class="btn" :class="todo.isDone ? 'btn-success' : 'btn-warning'" @click="done(index)">
							Done
						</button>
					</div>
					<div class="col-1">
						<button type="button" class="btn btn-danger" @click="del(index)">
							x
						</button>
					</div>
				</div>
			</li>
		</ul>
	</div>
</template>
<script>
export default {
	props:{
		todos:{
			type:Array,
			default:[]
		}
	},
	setup(props, {emit}){
	
		const del = (indexTodo) => {
			emit('deleteTodo',indexTodo)
		}

		const done = (indexTodo) => {
			emit('doneTodo',indexTodo)
		}

		return {
			del, done
		}
	}
}
</script>
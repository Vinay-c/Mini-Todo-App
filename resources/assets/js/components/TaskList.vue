<template>
    <div class='row'>
        <form action="#" @submit.prevent="createTask()">
            <div class="form-group">
                <input v-model="task.body" type="text" name="body" placeholder="Enter a new task..." class="form-control" autofocus>
            </div>
        </form>
      
        <ul class="list-group margintop20">
            <li v-if='list.length === 0'>There are no tasks yet!</li>
            <li class="list-group" v-for="(task, index) in list">
                 {{ task.body }}
            </li>
        </ul>
    </div>
</template>
<script>
    export default {
        data() {
            return {
                list: [],
                task: {
                    id: '',
                    body: ''
                }
            };
        },
        
        created() {
            this.fetchTaskList();
        },
        
        methods: {
            fetchTaskList() {
                axios.get('api/tasks').then((res) => {
                    this.list = res.data;
                });
            },
 
            createTask() {
                axios.post('api/tasks', this.task)
                    .then((res) => {
                        this.task.body = '';
                        this.edit = false;
                        this.fetchTaskList();
                    })
                    .catch((err) => console.error(err));
            },
        }
    }
</script>

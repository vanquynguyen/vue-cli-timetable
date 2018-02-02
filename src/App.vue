<template>
    <div class="container">
        <div class="row">
            <div class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3">
                <h1>Time table ({{tasks.length}})</h1>
                <hr>
                <br><br>
            </div>
        </div>
        <form @submit="addTask">
            <div class="form-group">
                <input v-model="newTask" class="form-control" placeholder="Tôi muốn...">
            </div>
        </form>
        <button class="btn btn-danger" @click="shuffle">Đổi chỗ</button>
        <div v-for="(check, index) in checks" v-bind:key="index">
            <input type="checkbox" v-model="checked" v-bind:value="check" /> 
            {{check}}
        </div>
        <span>Checked: {{ checked }}</span>
        <div class="row">
            <div class="form-group">
                <div class="searchable-container">
                    <transition-group name="list-complete" tag="p">
                        <div class="tasks col-xs-5 col-sm-5 col-md-2 col-lg-2" v-for="(task, index) in filteredTasks" v-bind:key="index">
                            <div class="info-block block-info clearfix">
                                <div class="square-box pull-left">
                                    <span class="glyphicon glyphicon-tags glyphicon-lg"></span>
                                    <span class="glyphicon glyphicon-trash glyphicon-lg" @click="removeTask(index)"></span>
                                </div>
                                <div data-toggle="buttons" class="btn-group bizmoduleselect">
                                    <label class="btn btn-default" id="check" @click="checkBox(index)">
                                        <div class="bizcontent">
                                            <input type="checkbox" autocomplete="off">
                                            <span class="glyphicon glyphicon-ok glyphicon-lg"></span>
                                            <h5>{{task.body}}</h5>
                                        </div>
                                    </label>
                                </div>
                            </div>
                            <br>
                        </div>
                    </transition-group>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data () {
            return {
                tasks: [
                    {body:"Ăn cơm", check:"Unchecked", completed: false},
                    {body:"Tán gái", check:"Unchecked", completed: false},
                    {body:"Lướt face", check:"Unchecked", completed: false},
                    {body:"Học Vue", check:"Unchecked", completed: false},
                    {body:"Ngắm gái", check:"Unchecked", completed: false},
                    {body:"Nhậu", check:"Unchecked", completed: false},
                    {body:"Học Laravel", check:"Unchecked", completed: false},
                    {body:"Ngủ", check:"Unchecked", completed: false},
                    {body:"Play game", check:"Unchecked", completed: false},
                    {body:"Play girl", check:"Unchecked", completed: false},
                ],

                checks: ['Checked', 'Unchecked'],
                newTask: '',
                checked: [],
            }
        },

        computed: {
            filteredTasks(){
                let filterTasks = this.tasks;

                $.each(this.checked, function(value, key){                       
                    filterTasks= filterTasks.filter(function(task){                            
                        return task.check == key;
                    })
                });

                return filterTasks;
            }
        },

        methods: {
        
            checkBox(indexTask) {
                this.tasks[indexTask].check = 'Checked';
            },

            addTask(e){
                e.preventDefault();
                this.tasks.push({
                    body: this.newTask,
                    check: "Unchecked",
                    completed: false
                });
                
                this.newTask = '';
            },

            removeTask(index) {
			    Vue.delete(this.tasks, index);
            },
            
            shuffle() {
                this.tasks = _.shuffle(this.tasks)
            },
        }
    }
</script>

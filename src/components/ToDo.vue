<template>
  <div class="container">
      <section class="to-do col s12">
          <h1 class="to-do__title">{{list.title}}</h1>
          <ul class="to-do__content">
              <ToDoItem v-for="(task, index) in list.tasks" :key = "index"
              :task = "task"
              v-on:removed = "removeTask"/>
          </ul>
          <form class="row to-do__control" @submit.prevent="">
                <div class="col s2">
                    <label>
                      <input type="checkbox"  v-model="newTask.isImportant" id="add-new-task__isImportant" />
                      <span>Важное</span>
                    </label>
                </div>
                <div class="col s5">
                      <div class="input-field">
                        <input class="autocomplete" v-model="newTask.title"  type="text" >
                        <label>Введите название задачи</label>
                      </div>
                </div>
                <div class="col s2">
                    <button v-on:click="addNewTask" type="submit" class="btn waves-effect waves-light purple lighten-2 col s12" >Добавить</button>
                </div>
                <div class="col s2">
                    <button v-on:click="logout()" type="submit" class="btn waves-effect waves-light purple lighten-2 col s12" >Выйти</button>
                </div>
          </form>
      </section>
  </div>
</template>

<script>
import ToDoItem from "@/components/t-ToDoItem"
import moment from 'moment'

export default {
    name: "Todo",
    data  () {
        return {
            newTask: {
                name: "",
                listTitle: "",
                date: 0,
                isImportant: false,
                isDone: false,
                isCreated: false
            }
        }
    },
    props: {
        list: Object
    },
    components: {
        ToDoItem
    },
    methods: {
      async logout () {
        await this.$store.dispatch('logout')
        this.$router.push('/login')
      },
        addNewTask  (){
            if (!Object.keys(this.list).length)
                this.$store.commit('setcurModal', {
                    name: "Список не выбран!",
                    isChoice: false,
                    submitText: "ОК",
                    contentText: `Выберите список`
                });
            else {
                this.newTask.date = moment().format("llll");
                this.newTask.listTitle = this.list.title;
                this.list.tasks.push(Object.assign({}, this.newTask));
                this.showCreatePopup(this.newTask.title, this.newTask.listTitle);
                this.newTask.title = "";
                this.newTask.isImportant = false;
            }
        },
        removeTask (tasktitle){
            this.list.tasks = this.list.tasks.filter((task) => {return task.title != tasktitle});
        },
        showCreatePopup  (tasktitle, listTitle){
            this.$store.commit('setcurModal', {
                title: "Задача добавлена",
                isChoice: false,
                submitText: "ОК",
                contentText: `Задача "` + tasktitle + `" добавлена в список "` + listTitle + `"`
            });
        }
    }
}
</script>


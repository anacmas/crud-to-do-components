<template>
  <div id="index">
    <nav class="orange darken-2">
      <div class="nav-wrapper"></div>
    </nav>
    <div class="container">
      <div class="section">
        <!--   Icon Section   -->
        <div class="row" v-show="!modoAdicionar">
          <!-- card de tarefa que repete para cada item do array -->
          <div
            v-for="(tarefa, index) in tasks"
            :key="index"
            class="card horizontal"
          >
            <div class="card-stacked">
              <div class="card-content">
                <p>{{ tarefa.title }}</p>
                <span class="projeto">{{ tarefa.project }}</span>
              </div>
              <div class="card-action">
                <a href="#">This is a link</a>
                <button @click="exclude(index)">Deletar</button>
                <button @click="edit(index)">Editar</button>
              </div>
            </div>
          </div>
        </div>
        <!-- NOVA TAREFA -->
        <div class="row" v-show="modoAdicionar">
          <div class="col s12">
            <div class="card horizontal">
              <div class="card-stacked">
                <div class="card-content">
                  <h2>
                    {{ indexCard != null ? 'Edit Task' : 'New Task' }}
                  </h2>
                  <form action="">
                    <div class="input-field col s12">
                      <input
                        v-model="title"
                        placeholder="Title"
                        id="title"
                        type="text"
                        class="validate"
                        required
                      />
                      <label for="title">Title</label>
                    </div>
                    <div class="input-field col s12">
                      <select id="project" v-model="project">
                        <option value="" disabled selected>
                          Choose your option
                        </option>
                        <option value="Estudos">Estudos</option>
                        <option value="Financeiro">Financeiro</option>
                        <option value="Trabalho">Trabalho</option>
                      </select>
                      <label for="project">Pick a Project</label>
                    </div>
                    <div class="input-field col s12">
                      <input
                        type="date"
                        class="datepicker"
                        id="dueTo"
                        v-model="date"
                      />
                      <label for="dueTo">Date due to</label>
                    </div>
                    <a
                      class="waves-effect waves-light btn-large"
                      id="btnSave"
                      @click="save"
                      >Save</a
                    >
                  </form>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="add">
        <a
          class="btn-floating btn-large waves-effect waves-light red"
          @click="adicionar"
          ><i class="fa-solid fa-plus"></i
        ></a>
      </div>
    </div>
    <footer class="page-footer orange darken-3">
      <div class="footer-copyright">
        <div class="container">© 2022</div>
      </div>
    </footer>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data: () => ({
    message: 'Olá Vue!',
    tasks: [],
    title: '',
    date: '',
    project: '',
    modoAdicionar: false,
    indexCard: null,
  }),
  methods: {
    getTasks() {
      fetch('http://localhost:3000/tasks')
        .then((response) => response.json())
        .then((tarefasJson) => {
          console.log(tarefasJson)
          this.tasks = tarefasJson
        })
    },
    adicionar() {
      this.modoAdicionar = true
    },
    save() {
      if (this.indexCard != null) {
        this.tasks[this.indexCard].title = this.title
        this.tasks[this.indexCard].date = this.date
        this.tasks[this.indexCard].project = this.project
        this.indexCard = null
      } else {
        this.tasks.push({
          title: this.title,
          date: this.date,
          project: this.project,
        })
      }
      this.modoAdicionar = false
      this.title = ''
      this.date = ''
      this.project = ''
    },
    exclude(index) {
      this.tasks.splice(index, 1)
    },
    edit(index) {
      this.title = this.tasks[index].title
      this.date = this.tasks[index].date
      this.project = this.tasks[index].project
      this.indexCard = index
      this.adicionar()
    },
  },
  created() {
    console.log('created')
    this.getTasks()
  },
  mounted() {
    console.log('montend')
  },
}
</script>

<style scoped>
select {
  display: inline;
  margin-top: 35px;
}
</style>

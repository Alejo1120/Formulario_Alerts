<template>
  <div>
    <v-container class="grey lighten-5">
      <v-row no-gutters>
        <v-col cols="12" sm="6" md="6" v-if="formAgregar">
          <v-container>
            <v-form @submit.prevent="Agregartarea">
              <v-card>
                <h2 class="text-center my-5">formulario</h2>
                <v-text-field
                  class="mx-2"
                  label="Name Task"
                  required
                  rows="1"
                  prepend-icon="fas fa-edit"
                  v-model="title"
                ></v-text-field>
                <v-textarea
                  class="mx-2"
                  label="Description"
                  rows="1"
                  prepend-icon="mdi-comment"
                  required
                  v-model="description"
                ></v-textarea>
                <v-card-actions>
                  <v-btn
                    color="success"
                    block
                    type="submit"
                    @click="Agregartarea"
                  >
                    Agregar
                  </v-btn>
                </v-card-actions>
              </v-card>
            </v-form>
          </v-container>
        </v-col>
        <v-col cols="12" sm="6" md="6" v-if="!formAgregar">
          <v-container>
            <v-form @submit.prevent="editarTarea">
              <v-card>
                <h2 class="text-center my-5">Editar</h2>
                <v-text-field
                  class="mx-2"
                  label="Name Task"
                  required
                  rows="1"
                  prepend-icon="fas fa-edit"
                  v-model="title"
                ></v-text-field>
                <v-textarea
                  class="mx-2"
                  label="Description"
                  rows="1"
                  prepend-icon="mdi-comment"
                  required
                  v-model="description"
                ></v-textarea>
                <v-card-actions>
                  <v-btn
                    color="warning"
                    block
                    type="submit"
                    @click="editarTarea"
                  >
                    Editar
                  </v-btn>
                </v-card-actions>
              </v-card>
            </v-form>
          </v-container>
        </v-col>
        <v-col cols="12" sm="6" md="6">
          <v-card class="mt-5" v-for="(item, index) in list_taskt" :key="index">
            <v-chip class="ma-2" color="success" label text-color="white">
              <v-icon left> mdi-label </v-icon>
              {{ item.title }}
            </v-chip>
            <v-card-text>
              <div class="text--primary">
                {{ item.description }}
              </div>
            </v-card-text>
            <v-card-actions>
              <v-btn color="warning" small @click="editar(index)">
                <v-icon dark class="mr-1"> mdi-pencil </v-icon> Editar
              </v-btn>
              <v-btn
                class="mx-5"
                color="error"
                small
                @click="eliminartarea(item.id)"
              >
                <v-icon dark class="mr-1"> mdi-delete </v-icon> Eliminar
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-col>
      </v-row>

      <div class="text-center">
        <v-snackbar v-model="snackbar" :timeout="timeout" :multi-line="multiLine">
          {{ text }}

          <template v-slot:action="{ attrs }">
            <v-btn color="red" text v-bind="attrs" @click="snackbar = false">
              Close
            </v-btn>
          </template>
        </v-snackbar>
      </div>
    </v-container>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",

  data: () => ({
    title: "",
    description: "",
    multiLine: true,
    snackbar: false,
    timeout: 2000,
    text: "",
    formAgregar: true,
    index_tarea: "",
    list_taskt: [
      {
        id: 1,
        title: "Tarea #1",
        description:
          "Lorem ipsum dolor sit amet consectetur adipisicing elit. Iusto, porro aspernatur enim ipsum fuga debitis, repellat quasi distinctio alias voluptate sunt quis labore magni? Vero similique quo molestias nihil inventore.",
      },
      {
        id: 2,
        title: "Tarea #2",
        description:
          "Lorem ipsum dolor sit amet consectetur adipisicing elit. Iusto, porro aspernatur enim ipsum fuga debitis, repellat quasi distinctio alias voluptate sunt quis labore magni? Vero similique quo molestias nihil inventore.",
      },
    ],
  }),

  methods: {
    Agregartarea() {
      console.log("form", this.title, this.description);
      if (this.title === "" || this.description === "") {
        console.log("campos vacios");
      } else {
        this.list_taskt.push({
          id: Date.now(),
          title: this.title,
          description: this.description,
        });
        this.snackbar= true;
        this.text = "Tarea Agregada"
        this.title = "";
        this.description = "";
      }
    },
    eliminartarea(id) {
      console.log("eliminado:", id);
      this.list_taskt = this.list_taskt.filter((e) => e.id != id);
      this.snackbar = true;
      this.text = "eliminada";
    },
    editar(index) {
      this.formAgregar = false;
      this.title = this.list_taskt[index].title;
      this.description = this.list_taskt[index].description;
      this.index_tarea = index;
    },
    editarTarea() {
      this.list_taskt[this.index_tarea].title = this.title;
      this.list_taskt[this.index_tarea].description = this.description;
      this.formAgregar = true;
      this.snackbar = true;
      this.text = " Tarea Editado";
      this.title = "";
      this.description = "";
    },
  },
};
</script>

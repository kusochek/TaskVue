<template>
  <ValidationObserver ref="form">
    <form @submit.prevent="onSubmit">
      <div class="form-row p-2">
        <div id="div-with-input" class="form-group col-md-6">
          <label for="inputEmail4">Name</label>
          <ValidationProvider
            :rules="{ required, alpha, min: 2, regex: /^[А-Яа-яЁё]+$/ }"
            name="First Name"
            v-slot="{ errors }"
            :bails="false"
            mode="passive"
          >
            <input
              v-model="firstName"
              placeholder="Введите имя"
              type="text"
              class="form-control"
              id="validationCustom01"
              required
            />
            <span id="span" ref="spanarea">{{ errors[0] }}</span>
          </ValidationProvider>
        </div>
        <div id="div-with-input" class="form-group col-md-6">
          <label for="inputPassword4">E-mail</label>
          <ValidationProvider
            name="E-mail"
            rules="required|email"
            v-slot="{ errors }"
            :bails="false"
            mode="passive"
          >
            <input
              v-model="email"
              placeholder="Введите e-mail"
              type="email"
              class="form-control"
              id="validationCustom01"
              required
            />
            <span id="span" ref="spanarea">{{ errors[0] }}</span>
          </ValidationProvider>
        </div>
      </div>
      <button
        id="submit"
        type="submit"
        class="btn btn-primary"
        data-toggle="modal"
        data-target="#exampleModal"
      >
        Submit
      </button>
      <modal name="modal">
        <div class="modal-header">
          <h5 class="modal-title" id="exampleModalLabel">
            This is just a notification that you are well done
          </h5>
          <button
            type="button"
            class="close"
            data-dismiss="modal"
            aria-label="Close"
          >
            <span aria-hidden="true">&times;</span>
          </button>
        </div>
        <div class="modal-body">
          Congratulations, you’ve submitted the form!
        </div>
        <div class="modal-footer">
          <button
            @click="hide"
            type="button"
            class="btn btn-secondary"
            data-dismiss="modal"
          >
            Close
          </button>
        </div>
      </modal>
    </form>
  </ValidationObserver>
</template>

<script>
export default {
  data: () => ({
    firstName: "",
    email: "",
  }),
  methods: {
    hide() {
      this.$modal.hide("modal");
    },
    onSubmit() {
      this.$refs.form.validate().then((success) => {
        if (!success) {
          const divInput = document.querySelectorAll("#div-with-input");
          for (let elem of divInput) {
            if (elem.querySelector("#span").innerText) {
              elem.querySelector("#validationCustom01").focus();
            }
          }
          return;
        }

        this.$modal.show("modal");

        this.firstName = this.email = "";

        this.$nextTick(() => {
          this.$refs.form.reset();
        });
      });
    },
  },
};
</script>

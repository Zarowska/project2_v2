<template>
  <div class="modal" id="createModal" tabindex="-1">
    <div class="modal-dialog">
      <div class="modal-content">
        <v-form @submit="onSubmit" :validation-schema="schema">
          <div class="modal-header">
            <h5 class="modal-title" id="createModalLabel">Add book</h5>
          </div>
          <div class="modal-body">
            <div class="mb-3">
              <label for="createauthor" class="form-label">author</label>
              <v-field
                name="author"
                id="createauthor"
                as="input"
                type="text"
                class="form-control"
              ></v-field>
              <error-message name="author" class="form-text text-danger" />
            </div>
            <div class="mb-3">
              <label for="createtitle" class="form-label">title</label>
              <v-field
                name="title"
                id="createtitle"
                as="input"
                type="text"
                class="form-control"
              ></v-field>
              <error-message name="title" class="form-text text-danger" />
            </div>
            <div class="mb-3">
              <label for="createdescription" class="form-label"
                >description</label
              >
              <v-field
                name="description"
                id="createdescription"
                as="input"
                type="text"
                class="form-control"
              ></v-field>
              <error-message name="description" class="form-text text-danger" />
            </div>
            <div class="mb-3">
              <label for="createimage" class="form-label"> URL image</label>
              <v-field
                name="image"
                id="createimage"
                as="input"
                type="url"
                class="form-control"
              ></v-field>
              <error-message name="image" class="form-text text-danger" />
            </div>

            <div class="mb-3">
              <label for="creategenre" class="form-label">genre</label>
              <v-field
                name="genre"
                id="creategenre"
                as="input"
                type="text"
                class="form-control"
              ></v-field>
              <error-message name="genre" class="form-text text-danger" />
            </div>

            <div class="mb-3">
              <label for="createpages" class="form-label">pages</label>
              <v-field
                name="pages"
                as="input"
                type="number"
                id="createpages"
                min="0"
                class="form-control"
              ></v-field>
              <error-message name="pages" class="form-text text-danger" />
            </div>

            <div class="mb-3">
              <label for="createlanguage" class="form-label">language</label>
              <v-field
                name="language"
                id="createlanguage"
                as="input"
                type="text"
                class="form-control"
              ></v-field>
              <error-message name="language" class="form-text text-danger" />
            </div>

            <div class="mb-3">
              <label for="createprice" class="form-label">price</label>
              <v-field
                name="price"
                id="createprice"
                as="input"
                type="number"
                min="0"
                class="form-control"
              ></v-field>
              <error-message name="price" class="form-text text-danger" />
            </div>
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-secondary" @click="close">
              Close
            </button>
            <button type="submit" class="btn btn-primary">Add</button>
          </div>
        </v-form>
      </div>
    </div>
  </div>
</template>

<script>
import { Field, Form } from "vee-validate";

import * as yup from "yup";

export default {
  name: "AddBooksModal",
  components: {
    VForm: Form,
    VField: Field,
  },
  data() {
    const schema = yup.object().shape({
      author: yup.string().required(),
      title: yup.string().required(),
      description: yup.string().required(),
      image: yup.string().url().required(),
      genre: yup.string().required(),
      pages: yup.number().required().positive(),
      language: yup.string().required(),
      price: yup.number().required().positive(),
    });

    return {
      schema,
    };
  },
  props: {},
  methods: {
    close() {
      this.$emit("close");
    },
    onSubmit(values, { resetForm }) {
      this.$emit("submit", values);
      resetForm();
    },
  },
};
</script>

<style scoped>
</style>

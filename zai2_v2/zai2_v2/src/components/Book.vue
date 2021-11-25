<template>
  <div class="card" style="max-width: 50%">
    <div
      class="col-sm-3 col-md-6 col-lg-4 text-right"
      style="padding: 10px"
    ></div>

    <div class="row">
      <div class="text-center">
        <h4 class="product-name">
          <strong>{{ book.author }}</strong>
        </h4>
      </div>

      <div class="text-center">
        <h3 class="product-name">
          <strong>{{ book.title }}</strong>
        </h3>
      </div>

      <div class="text-center">
        <img class="img-fluid img-thumbnail" :src="book.image" />
      </div>

      <div class="text-left">
        <h6 class="product-name">
          <small>{{ book.description }}</small>
        </h6>
      </div>

      <div class="text-left">
        <h6 class="product-name">
          <strong>Price: </strong>{{ book.price }} pln
        </h6>
      </div>

      <div class="text-left">
        <h6 class="product-name"><strong>Genre: </strong>{{ book.genre }}</h6>
      </div>

      <div class="text-left">
        <h6 class="product-name"><strong>Pages: </strong>{{ book.pages }}</h6>
      </div>

      <div class="text-left">
        <h6 class="product-name">
          <strong>Language: </strong>{{ book.language }}
        </h6>
      </div>

      <div class="text-center">
                <button
          class="btn btn-danger"
          @click.prevent="$emit('remove', book.id)"
        >
          Delete
        </button>
      </div>
      <div class="text-center">
        <br />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Book",
  props: {
    book: {
      required: true,
      type: Object,
    },
  },
};
</script>

<style scoped>
</style>

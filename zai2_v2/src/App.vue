<template>
  <nav>
    <ul class="nav nav-pills">
      <li class="nav-item dropdown">
        <a
          class="nav-link dropdown-toggle"
          data-bs-toggle="dropdown"
          href="#"
          role="button"
          aria-expanded="false"
          >Sort by</a
        >
        <ul class="dropdown-menu">
          <li>
            <a
              class="dropdown-item"
              href="#"
              @click="sortBooks('DESC')"
              :class="{ active: sortKey === 'DESC' }"
              >Price low to hight</a
            >
          </li>

          <li>
            <a
              class="dropdown-item"
              href="#"
              @click="sortBooks('ASC')"
              :class="{ active: sortKey === 'ASC' }"
              >Price hight to low</a
            >
          </li>
          <li>
            <a
              class="dropdown-item"
              href="#"
              @click="sortBooks('')"
              :class="{ active: sortKey === '' }"
              >Default</a
            >
          </li>
        </ul>
      </li>
      <li class="nav-item">
        <a class="nav-link disabled" href="#" tabindex="-1" aria-disabled="true"
          >Price:</a
        >
      </li>
      <v-form class="d-flex" @submit="onPriceSubmit">
        <li class="nav-item ml-0 ml-lg-2">
          <v-field
            name="priceFrom"
            as="input"
            type="number"
            min="0"
            v-model="priceMinimum"
            class="form-control mr-1"
            placeholder="min"
          ></v-field>
        </li>
        <li class="nav-item ml-2">
          <v-field
            name="priceTo"
            as="input"
            type="number"
            min="0"
            v-model="priceMaximum"
            class="form-control"
            placeholder="max"
          ></v-field>
        </li>
        <li class="nav-item ml-2">
          <button
            class="btn btn-outline-primary text-nowrap ml-1"
            type="submit"
          >
            Filter
          </button>
        </li>
      </v-form>

      <li class="nav-item mt-2 mt-lg-0">
        <button
          class="btn btn-outline-primary ml-1"
          @click="resetPriceFilter()"
        >
          Reset
        </button>
      </li>
    </ul>
  </nav>

  <nav>
    <form class="d-flex">
      <button
        type="button"
        class="btn btn-outline-success"
        @click="this.createModal.show()"
      >
        Add book
      </button>
    </form>
  </nav>

  <main class="d-flex flex-column align-items-center">
    <Book
      v-for="book in filterBooks"
      :key="book.id"
      :book="book"
      @remove="onDeleteSubmit"
    />
  </main>

  <AddBooksModal
    :categories="categories"
    @close="this.createModal.hide()"
    @submit="onSubmit"
  />
</template>

<script>
import Book from "./components/Book.vue";
import AddBooksModal from "./components/AddBooks.vue";

import { reactive } from "vue";

import { Modal } from "bootstrap";
import "bootstrap/dist/css/bootstrap.min.css";

import { Field, Form } from "vee-validate";

const store = {
  state: reactive({
    index: 5,

    books: [
      {
        id: 0,
        author: "James Joyce",
        title: "Ulysses",
        description:
          "Ulysses chronicles the passage of Leopold Bloom through Dublin during an ordinary day, June 16, 1904. The title parallels and alludes to Odysseus (Latinised into Ulysses), the hero of Homer s Odyssey (e.g., the correspondences between Leopold Bloom and Odysseus, Molly Bloom and Penelope, and Stephen Dedalus and Telemachus). Joyce fans worldwide now celebrate June 16 as Bloomsday.",
        image:
          "https://upload.wikimedia.org/wikipedia/commons/a/ab/JoyceUlysses2.jpg",
        genre: "Modernist novel",
        pages: 730,
        language: "English",
        price: 41,
      },
      {
        id: 1,
        author: "Miguel de Cervantes",
        title: "Don Quixote",
        description:
          "Alonso Quixano, a retired country gentleman in his fifties, lives in an unnamed section of La Mancha with his niece and a housekeeper. He has become obsessed with books of chivalry, and believes their every word to be true, despite the fact that many of the events in them are clearly impossible. Quixano eventually appears to other people to have lost his mind from little sleep and food and because of so much reading.",
        image:
          "https://images-na.ssl-images-amazon.com/images/I/41EzXfowWeL._SX321_BO1,204,203,200_.jpg",
        genre: "Novel",
        pages: 1056,
        language: "English",
        price: 40,
      },
      {
        id: 2,
        author: "Gustave Flaubert",
        title: "Madame Bovary",
        description:
          'For daring to peer into the heart of an adulteress and enumerate its contents with profound dispassion, the author of Madame Bovary was tried for "offenses against morality and religion.',
        image:
          "https://upload.wikimedia.org/wikipedia/commons/thumb/9/9f/Madame_Bovary_1857_%28hi-res%29.jpg/220px-Madame_Bovary_1857_%28hi-res%29.jpg",
        genre: "Tragedy",
        pages: 448,
        language: "French",
        price: 31,
      },
      {
        id: 3,
        author: "Vladimir Nabokov",
        title: "Lolita",
        description:
          "The book is internationally famous for its innovative style and infamous for its controversial subject: the protagonist and unreliable narrator, middle aged Humbert Humbert, becomes obsessed and sexually involved with a twelve-year-old girl named Dolores Haze.",
        image:
          "https://upload.wikimedia.org/wikipedia/commons/thumb/5/57/Lolita_1955.JPG/220px-Lolita_1955.JPG",
        genre: "Novel",
        pages: 336,
        language: "English",
        price: 100,
      },
      {
        id: 4,
        author: "Witold Gombrowicz",
        title: "Ferdydurke",
        description:
          'Ferdydurke is a novel by the Polish writer Witold Gombrowicz, published in 1937. Gombrowicz himself wrote of his novel that it is not "... a satire on some social class, nor a nihilistic attack on culture... We live in an era of violent changes, of accelerated development, in which settled forms are breaking under life s pressure... ',
        image:
          "https://ecsmedia.pl/c/ferdydurke-lekcja-literatury-w-iext80776041.jpg",
        genre: "Novel",
        pages: 296,
        language: "Polish",
        price: 23,
      },
    ],
  }),

  addBook(book) {
    book.id = this.state.index;
    this.state.index++;
    this.state.books.push(book);
  },

  saveBooks(book) {
    let index = this.state.books.findIndex((p) => p.id === book.id);
    this.state.books[index].author = book.author;
    this.state.books[index].title = book.title;
    this.state.books[index].description = book.description;
    this.state.books[index].image = book.image;
    this.state.books[index].genre = book.genre;
    this.state.books[index].pages = book.pages;
    this.state.books[index].language = book.language;
    this.state.books[index].price = book.price;
  },

  deleteBook(id) {
    this.state.books.splice(
      this.state.books.findIndex((book) => book.id === id),
      1
    );
  },
};

export default {
  name: "App",
  components: {
    Book,
    AddBooksModal,

    VForm: Form,
    VField: Field,
  },
  data() {
    return {
      store: store.state,
      sortKey: "",
      filterKey: "",
      priceMinimum: "",
      priceMaximum: "",
      filterBooks: store.state.books,
      createModal: null,
    };
  },
  mounted() {
    this.createModal = new Modal(document.getElementById("createModal"), {
      backdrop: "static",
    });
  },
  methods: {
    sortBooks(s) {
      this.sortKey = s;
      this.filterBooks.sort(
        function (a, b) {
          if (s == "") {
            return parseInt(a.id) > parseInt(b.id) ? 1 : -1;
          }
          if (s == "ASC") {
            return parseInt(a.price) < parseInt(b.price) ? 1 : -1;
          }

          if (s == "DESC") {
            return parseInt(a.price) > parseInt(b.price) ? 1 : -1;
          }
        }.bind(this)
      );
    },

    doFilterBooks() {
      let p = null;
      p = this.store.books;
      if (this.priceMinimum) {
        p = p.filter((book) => book.price >= this.priceMinimum);
      }
      if (this.priceMaximum) {
        p = p.filter((book) => book.price <= this.priceMaximum);
      }
      this.filterBooks = p;
      this.sortBooks(this.sortKey);
    },

    onPriceSubmit(values) {
      if (values.priceFrom) {
        this.priceMinimum = values.priceFrom;
      } else {
        this.priceMinimum = "";
      }
      if (values.priceFrom) {
        this.priceMinimum = values.priceFrom;
      } else {
        this.priceMinimum = "";
      }
      if (values.priceTo) {
        this.priceMaximum = values.priceTo;
      } else {
        this.priceMaximum = "";
      }
      this.doFilterBooks();
    },

    resetPriceFilter() {
      this.priceMinimum = "";
      this.priceMaximum = "";
      this.doFilterBooks();
    },

    onDeleteSubmit(id) {
      store.deleteBook(id);
      this.doFilterBooks();
    },

    onSubmit(values) {
      this.createModal.hide();
      store.addBook(values);
      this.doFilterBooks();
    },
  },

  computed: {},
};
</script>

<style>
</style>
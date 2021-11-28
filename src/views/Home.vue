<template>
  <div class="home">
    <h1 class="subheading black--text pa-5">Books</h1>
    <v-container>
      <v-card flat class="pa-8" v-for="book in books" :key="book.title">
        <v-layout row wrap>
          <v-flex xs4 md1 class="ma-2">
            <div class="caption grey--text">Image</div>
            <v-img :src="book.image" :lazy-src="book.image"
            ></v-img>
          </v-flex>
          <v-flex xs4 md2 class="ma-2">
            <div class="caption grey--text">Title</div>
            <div>{{ book.title }}</div>
          </v-flex>
          <v-flex xs4 md2 class="ma-2">
            <div class="caption grey--text">Author</div>
            <div>{{ book.author }}</div>
          </v-flex>
          <v-flex xs12 md2 class="ma-2">
            <div class="caption grey--text">Description</div>
            <div>{{ book.description }}</div>
          </v-flex>
          <v-flex xs4 md2 class="ma-2">
            <div class="caption grey--text">Publisher</div>
            <div>{{ book.publisher }}</div>
          </v-flex>
          <v-flex xs4 md1 class="ma-2">
            <div class="caption grey--text">Year of publishing</div>
            <div>{{ book.year }}</div>
          </v-flex>
          <v-flex xs4 md1 class="ma-2">
            <div class="caption grey--text">Price</div>
            <div>{{ book.price }}</div>
          </v-flex>
        </v-layout>
      </v-card>
    </v-container>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        books: [
          {image: "https://s.lubimyczytac.pl/upload/books/314000/314304/499287-352x500.jpg", title: "Finansowy Ninja", author: "Michał Szafrański", description: "Finansowy ninja to esencja wiedzy autora i jego doświadczenia na temat finansów osobistych. To podręcznik, który uzupełnić ma braki w edukacji szkolnej. Wyjaśnia podstawowe, ale nieoczywiste zasady, daje proste podpowiedzi dotyczące mądrego posługiwania się produktami finansowymi i przestrzega przed zagrożeniami dla naszych pieniędzy.", publisher: "Michał Szafrański", year: "2016", price:"50.00"},
          {image: "https://s.lubimyczytac.pl/upload/books/4940000/4940535/844684-352x500.jpg", title: "Finansowa Forteca", author: "Marcin Iwuć", description: "Chcesz inwestować, ale nie wiesz, od czego zacząć? Czujesz opór przed nudnymi książkami? Masz dość akademickich tomiszczy, naszpikowanych trudnymi zagadnieniami, z których nic nie wynika? Zamiast próbować „uczyć się inwestować”, zbuduj krok po kroku Finansową Fortecę, która skutecznie ochroni majątek Twój i Twojej rodziny!", publisher: "FBO Marcin Iwuć", year: "2020", price:"80.00"},
        ]
      }
    }
  }
</script>

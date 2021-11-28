<template>
  <div class="home">
    <h1 class="subheading black--text pa-5">Books</h1>
    <v-container>

      <v-layout row class="mb-3 pa-8">
        <span class="heading grey--text mr-5">Sort by:</span>
        <v-btn small flat class="mx-1" @click="sortBy('title')">
          <v-icon left small>title</v-icon>
          <span>Title</span>
        </v-btn>
        <v-btn small flat class="mx-1" @click="sortBy('author')">
          <v-icon left small>person</v-icon>
          <span>Author</span>
        </v-btn>
        <v-btn small flat class="mx-1" @click="sortBy('publisher')">
          <v-icon left small>business</v-icon>
          <span>Publisher</span>
        </v-btn>
        <v-btn small flat class="mx-1" @click="sortBy('year')">
          <v-icon left small>schedule</v-icon>
          <span>Year of publishing</span>
        </v-btn>
        <v-btn small flat class="mx-1" @click="sortBy('price')">
          <v-icon left small>payments</v-icon>
          <span>Prices</span>
        </v-btn>
        <v-spacer></v-spacer>
        <span class="heading grey--text mr-5">Filter by price:</span>
        <v-text-field label="From" hide-details="auto" class="me-5" @keypress="isNumber($event)"></v-text-field>
        <v-text-field label="To" hide-details="auto" class="ms-5" @keypress="isNumber($event)"></v-text-field>
      </v-layout>

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
          {image: "https://s.lubimyczytac.pl/upload/books/4942000/4942534/930750-352x500.jpg", title: "Diuna", author: "Frank Herbert", description: "Arrakis, zwana Diuną, to jedyne we wszechświecie źródło melanżu. Z rozkazu Padyszacha Imperatora planetę przejmują Atrydzi, zaciekli wrogowie władających nią dotychczas Harkonnenów. Zwycięstwo księcia Leto Atrydy jest jednak pozorne – przejęcie planety ukartowano. W odpowiedzi na atak Imperium i Harkonnenów dziedzic rodu Atrydów Paul staje na czele rdzennych mieszkańców Diuny i sięga po imperialny tron. Oszałamiające połączenie przygody oraz mistycyzmu, ekologii i polityki.", publisher: "Rebis", year: "2021", price:"40.00"},
          {image: "https://s.lubimyczytac.pl/upload/books/4988000/4988766/936150-352x500.jpg", title: "Przepaść", author: "Remigiusz Mróz", description: "W Tatrach Zachodnich dochodzi do zaginięcia młodej kobiety z Warszawy. Ostatnim razem widziana była przez turystów w okolicy Trzydniowiańskiego Wierchu, a potem przepadła bez wieści. Był to jej pierwszy wyjazd w góry, miała zamiar spędzić w nich tylko weekend, a w Zakopanem pojawiła się sama – mimo to wedle ustaleń policji szła czerwonym szlakiem w towarzystwie czterech mężczyzn. Sprawę prowadzi Wiktor Forst, który odtwarza trasę dziewczyny z nadzieją, że odnajdzie ją żywą. Nie trafia na żadne poszlaki, odkrywa jednak w Tatrach coś, co rzuci nowe światło na zbrodnię sprzed lat…", publisher: "Filia", year: "2021", price:"44.00"},
          {image: "https://s.lubimyczytac.pl/upload/books/4897000/4897113/763444-352x500.jpg", title: "Kasztanowy ludzik", author: "Soren Sveistrup", description: "Psychopata terroryzuje Kopenhagę - krwawo morduje swe ofiary, a na miejscach zbrodni pozostawia ręcznie zrobione kasztanowe ludziki.", publisher: "W.A.B.", year: "2019", price:"55.00"},
          {image: "https://s.lubimyczytac.pl/upload/books/4936000/4936081/934525-352x500.jpg", title: "Kajś. Opowieść o Górnym Śląsku", author: "Zbigniew Rokita", description: "Przez większość życia uważałem Ślązaków za jaskiniowców z kilofem i roladą. Swoją śląskość wypierałem. W podstawówce pani Chmiel grała nam na akordeonie Rotę, a ja nie miałem pojęcia, że ów plujący w twarz Niemiec z pieśni był moim przodkiem.", publisher: "Czarne", year: "2020", price:"32.00"},
          {image: "https://s.lubimyczytac.pl/upload/books/4963000/4963549/920062-352x500.jpg", title: "Święto ognia", author: "Jakub Małecki", description: "Tata tańczy za szybą. Podnosi rozmazane przez deszcz ręce i kołysze rozmazaną przez deszcz głową, ma zamknięte oczy i ja w końcu też zamykam swoje, i śpię, i budzę się rano, i znowu jesteśmy razem.", publisher: "Sine Qua Non", year: "2021", price:"41.00"},
          {image: "https://s.lubimyczytac.pl/upload/books/4882000/4882986/866945-352x500.jpg", title: "27 śmierci Toby’ego Obeda", author: "Joanna Gierak-Onoszko", description: "To też jest Kanada: siedem zapałek w słoiku, sny o czubkach drzew, powiewające na wietrze czerwone suknie, dzieci odbierane rodzicom o świcie. I ludzie, którzy nie mówią, że są absolwentami szkół z internatem. Mówią: jesteśmy ocaleńcami. Przetrwaliśmy.", publisher: "Dowody na Istnienie", year: "2019", price:"56.00"},
        ]
      }
    },
    methods: {
      sortBy(prop) {
        this.books.sort((a,b) => a[prop] < b[prop] ? -1 : 1)
      },
      filterByPrice(from, to) {
        this.books.filter(
            function (book) {
              return (book['price'] >= from && book['price'] <= to)
            }
        )
      },
      isNumber: function(evt) {
        evt = (evt) ? evt : window.getEvents;
        var charCode = (evt.which) ? evt.which : evt.keyCode;
        if ((charCode > 31 && (charCode < 48 || charCode > 57)) && charCode !== 46) {
          evt.preventDefault();
        } else {
          return true;
        }
      }
    }
  }
</script>

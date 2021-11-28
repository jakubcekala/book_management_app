<template>
  <div class="home">
    <h1 class="subheading black--text pa-5">Books</h1>
    <v-container>

      //Add new dialog
      <v-dialog v-model="dialogNew">
        <template v-slot:activator="{ on, attrs}">
          <v-btn
              color="primary"
              v-bind="attrs"
              v-on="on"
          >Add new book</v-btn>
        </template>
        <template>
          <v-card>
            <v-toolbar
                color="primary"
                dark
            >Add a new book</v-toolbar>
            <v-card-text>
              <v-form class="px-5" ref="form">
                <v-text-field label="Title" v-model="bookItem.title" :rules="bookItem.inputRules"></v-text-field>
                <v-text-field label="Author" v-model="bookItem.author" :rules="bookItem.inputRules"></v-text-field>
                <v-textarea label="Description" v-model="bookItem.description" :rules="bookItem.inputRules"></v-textarea>
                <v-text-field label="Publisher" v-model="bookItem.publisher" :rules="bookItem.inputRules"></v-text-field>
                <v-text-field label="Year of publishing" v-model="bookItem.year" :rules="bookItem.inputRules"></v-text-field>
                <v-text-field label="Price" v-model="bookItem.price" :rules="bookItem.priceRule"></v-text-field>
                <v-text-field label="Image url" v-model="bookItem.image" :rules="bookItem.urlRules"></v-text-field>
                <v-btn class="success" @click="submit">Add book</v-btn>
              </v-form>
            </v-card-text>
          </v-card>
        </template>
      </v-dialog>

      //Edit dialog
      <v-dialog v-model="dialogEdit" :retain-focus="false">
        <template>
          <v-card>
            <v-toolbar
                color="primary"
                dark
            >Edit the book</v-toolbar>
            <v-card-text>
              <v-form class="px-5" ref="editForm">
                <v-text-field label="Title" v-model="bookItem.title" :rules="bookItem.inputRules"></v-text-field>
                <v-text-field label="Author" v-model="bookItem.author" :rules="bookItem.inputRules"></v-text-field>
                <v-textarea label="Description" v-model="bookItem.description" :rules="bookItem.inputRules"></v-textarea>
                <v-text-field label="Publisher" v-model="bookItem.publisher" :rules="bookItem.inputRules"></v-text-field>
                <v-text-field label="Year of publishing" v-model="bookItem.year" :rules="bookItem.inputRules"></v-text-field>
                <v-text-field label="Price" v-model="bookItem.price" :rules="bookItem.priceRule"></v-text-field>
                <v-text-field label="Image url" v-model="bookItem.image" :rules="bookItem.urlRules"></v-text-field>
                <v-btn class="success" @click="updateUser(index)">Submit changes</v-btn>
              </v-form>
            </v-card-text>
          </v-card>
        </template>
      </v-dialog>

      //Sorting row
      <v-layout row class="mb-3 pa-8">
        <span class="heading grey--text mr-5">Sort by:</span>
        <v-btn small class="mx-1" @click="sortBy('title')">
          <v-icon left small>title</v-icon>
          <span>Title</span>
        </v-btn>
        <v-btn small class="mx-1" @click="sortBy('author')">
          <v-icon left small>person</v-icon>
          <span>Author</span>
        </v-btn>
        <v-btn small class="mx-1" @click="sortBy('publisher')">
          <v-icon left small>business</v-icon>
          <span>Publisher</span>
        </v-btn>
        <v-btn small class="mx-1" @click="sortBy('year')">
          <v-icon left small>schedule</v-icon>
          <span>Year of publishing</span>
        </v-btn>
        <v-btn small class="mx-1" @click="sortBy('price')">
          <v-icon left small>payments</v-icon>
          <span>Prices</span>
        </v-btn>
      </v-layout>
      <v-layout column class="mb-3 pa-8">
        <span class="heading grey--text">Filter by price:</span>
        <v-flex xs4 md1 class="ma-2">
          <v-text-field label="From" hide-details="auto" class="mx-5" @keypress="isNumber($event)" v-model="priceFilter.valueFrom"></v-text-field>
          <v-text-field label="To" hide-details="auto" class="mx-5" @keypress="isNumber($event)" v-model="priceFilter.valueTo"></v-text-field>
        </v-flex>
        <v-flex xs4 md1 class="ma-2">
          <v-btn class="mx-1" color="primary" @click="filterByPrice">Filter</v-btn>
          <v-btn color="primary" @click="resetFilters">Clear filter</v-btn>
        </v-flex>
      </v-layout>

      //Tables with books
      <v-card class="pa-8" v-for="(book, index) in books" :key="book.title">
        <v-layout row wrap>
          <v-flex xs4 md1 class="ma-2">
            <div class="caption grey--text">Image</div>
            <v-img :src="book.image" :lazy-src="book.image"
            ></v-img>
          </v-flex>
          <v-flex xs4 md2 class="ma-2">
            <div class="caption grey--text">Title</div>
            <div>{{ book.title }}</div>
            <div class="caption grey--text">Author</div>
            <div>{{ book.author }}</div>
          </v-flex>
          <v-flex xs12 md3 class="ma-2">
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
            <v-btn class="error mt-1" @click="removeItem(index)">
              <v-icon>delete</v-icon>
            </v-btn>
            <v-btn class="warning mt-1" v-bind="attrs" v-on="on" @click="provideBookData(index)">
              <v-icon>edit</v-icon>
            </v-btn>
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
        booksRepository: [
          {image: "https://s.lubimyczytac.pl/upload/books/314000/314304/499287-352x500.jpg", title: "Finansowy Ninja", author: "Michał Szafrański", description: "Finansowy ninja to esencja wiedzy autora i jego doświadczenia na temat finansów osobistych. To podręcznik, który uzupełnić ma braki w edukacji szkolnej. Wyjaśnia podstawowe, ale nieoczywiste zasady, daje proste podpowiedzi dotyczące mądrego posługiwania się produktami finansowymi i przestrzega przed zagrożeniami dla naszych pieniędzy.", publisher: "Michał Szafrański", year: "2016", price:"50.00"},
          {image: "https://s.lubimyczytac.pl/upload/books/4940000/4940535/844684-352x500.jpg", title: "Finansowa Forteca", author: "Marcin Iwuć", description: "Chcesz inwestować, ale nie wiesz, od czego zacząć? Czujesz opór przed nudnymi książkami? Masz dość akademickich tomiszczy, naszpikowanych trudnymi zagadnieniami, z których nic nie wynika? Zamiast próbować „uczyć się inwestować”, zbuduj krok po kroku Finansową Fortecę, która skutecznie ochroni majątek Twój i Twojej rodziny!", publisher: "FBO Marcin Iwuć", year: "2020", price:"80.00"},
          {image: "https://s.lubimyczytac.pl/upload/books/4942000/4942534/930750-352x500.jpg", title: "Diuna", author: "Frank Herbert", description: "Arrakis, zwana Diuną, to jedyne we wszechświecie źródło melanżu. Z rozkazu Padyszacha Imperatora planetę przejmują Atrydzi, zaciekli wrogowie władających nią dotychczas Harkonnenów. Zwycięstwo księcia Leto Atrydy jest jednak pozorne – przejęcie planety ukartowano. W odpowiedzi na atak Imperium i Harkonnenów dziedzic rodu Atrydów Paul staje na czele rdzennych mieszkańców Diuny i sięga po imperialny tron. Oszałamiające połączenie przygody oraz mistycyzmu, ekologii i polityki.", publisher: "Rebis", year: "2021", price:"40.00"},
          {image: "https://s.lubimyczytac.pl/upload/books/4988000/4988766/936150-352x500.jpg", title: "Przepaść", author: "Remigiusz Mróz", description: "W Tatrach Zachodnich dochodzi do zaginięcia młodej kobiety z Warszawy. Ostatnim razem widziana była przez turystów w okolicy Trzydniowiańskiego Wierchu, a potem przepadła bez wieści. Był to jej pierwszy wyjazd w góry, miała zamiar spędzić w nich tylko weekend, a w Zakopanem pojawiła się sama – mimo to wedle ustaleń policji szła czerwonym szlakiem w towarzystwie czterech mężczyzn. Sprawę prowadzi Wiktor Forst, który odtwarza trasę dziewczyny z nadzieją, że odnajdzie ją żywą. Nie trafia na żadne poszlaki, odkrywa jednak w Tatrach coś, co rzuci nowe światło na zbrodnię sprzed lat…", publisher: "Filia", year: "2021", price:"44.00"},
          {image: "https://s.lubimyczytac.pl/upload/books/4897000/4897113/763444-352x500.jpg", title: "Kasztanowy ludzik", author: "Soren Sveistrup", description: "Psychopata terroryzuje Kopenhagę - krwawo morduje swe ofiary, a na miejscach zbrodni pozostawia ręcznie zrobione kasztanowe ludziki.", publisher: "W.A.B.", year: "2019", price:"55.00"},
          {image: "https://s.lubimyczytac.pl/upload/books/4936000/4936081/934525-352x500.jpg", title: "Kajś. Opowieść o Górnym Śląsku", author: "Zbigniew Rokita", description: "Przez większość życia uważałem Ślązaków za jaskiniowców z kilofem i roladą. Swoją śląskość wypierałem. W podstawówce pani Chmiel grała nam na akordeonie Rotę, a ja nie miałem pojęcia, że ów plujący w twarz Niemiec z pieśni był moim przodkiem.", publisher: "Czarne", year: "2020", price:"32.00"},
          {image: "https://s.lubimyczytac.pl/upload/books/4963000/4963549/920062-352x500.jpg", title: "Święto ognia", author: "Jakub Małecki", description: "Tata tańczy za szybą. Podnosi rozmazane przez deszcz ręce i kołysze rozmazaną przez deszcz głową, ma zamknięte oczy i ja w końcu też zamykam swoje, i śpię, i budzę się rano, i znowu jesteśmy razem.", publisher: "Sine Qua Non", year: "2021", price:"41.00"},
          {image: "https://s.lubimyczytac.pl/upload/books/4882000/4882986/866945-352x500.jpg", title: "27 śmierci Toby’ego Obeda", author: "Joanna Gierak-Onoszko", description: "To też jest Kanada: siedem zapałek w słoiku, sny o czubkach drzew, powiewające na wietrze czerwone suknie, dzieci odbierane rodzicom o świcie. I ludzie, którzy nie mówią, że są absolwentami szkół z internatem. Mówią: jesteśmy ocaleńcami. Przetrwaliśmy.", publisher: "Dowody na Istnienie", year: "2019", price:"56.00"},
        ],
        books: [
          {image: "https://s.lubimyczytac.pl/upload/books/314000/314304/499287-352x500.jpg", title: "Finansowy Ninja", author: "Michał Szafrański", description: "Finansowy ninja to esencja wiedzy autora i jego doświadczenia na temat finansów osobistych. To podręcznik, który uzupełnić ma braki w edukacji szkolnej. Wyjaśnia podstawowe, ale nieoczywiste zasady, daje proste podpowiedzi dotyczące mądrego posługiwania się produktami finansowymi i przestrzega przed zagrożeniami dla naszych pieniędzy.", publisher: "Michał Szafrański", year: "2016", price:"50.00"},
          {image: "https://s.lubimyczytac.pl/upload/books/4940000/4940535/844684-352x500.jpg", title: "Finansowa Forteca", author: "Marcin Iwuć", description: "Chcesz inwestować, ale nie wiesz, od czego zacząć? Czujesz opór przed nudnymi książkami? Masz dość akademickich tomiszczy, naszpikowanych trudnymi zagadnieniami, z których nic nie wynika? Zamiast próbować „uczyć się inwestować”, zbuduj krok po kroku Finansową Fortecę, która skutecznie ochroni majątek Twój i Twojej rodziny!", publisher: "FBO Marcin Iwuć", year: "2020", price:"80.00"},
          {image: "https://s.lubimyczytac.pl/upload/books/4942000/4942534/930750-352x500.jpg", title: "Diuna", author: "Frank Herbert", description: "Arrakis, zwana Diuną, to jedyne we wszechświecie źródło melanżu. Z rozkazu Padyszacha Imperatora planetę przejmują Atrydzi, zaciekli wrogowie władających nią dotychczas Harkonnenów. Zwycięstwo księcia Leto Atrydy jest jednak pozorne – przejęcie planety ukartowano. W odpowiedzi na atak Imperium i Harkonnenów dziedzic rodu Atrydów Paul staje na czele rdzennych mieszkańców Diuny i sięga po imperialny tron. Oszałamiające połączenie przygody oraz mistycyzmu, ekologii i polityki.", publisher: "Rebis", year: "2021", price:"40.00"},
          {image: "https://s.lubimyczytac.pl/upload/books/4988000/4988766/936150-352x500.jpg", title: "Przepaść", author: "Remigiusz Mróz", description: "W Tatrach Zachodnich dochodzi do zaginięcia młodej kobiety z Warszawy. Ostatnim razem widziana była przez turystów w okolicy Trzydniowiańskiego Wierchu, a potem przepadła bez wieści. Był to jej pierwszy wyjazd w góry, miała zamiar spędzić w nich tylko weekend, a w Zakopanem pojawiła się sama – mimo to wedle ustaleń policji szła czerwonym szlakiem w towarzystwie czterech mężczyzn. Sprawę prowadzi Wiktor Forst, który odtwarza trasę dziewczyny z nadzieją, że odnajdzie ją żywą. Nie trafia na żadne poszlaki, odkrywa jednak w Tatrach coś, co rzuci nowe światło na zbrodnię sprzed lat…", publisher: "Filia", year: "2021", price:"44.00"},
          {image: "https://s.lubimyczytac.pl/upload/books/4897000/4897113/763444-352x500.jpg", title: "Kasztanowy ludzik", author: "Soren Sveistrup", description: "Psychopata terroryzuje Kopenhagę - krwawo morduje swe ofiary, a na miejscach zbrodni pozostawia ręcznie zrobione kasztanowe ludziki.", publisher: "W.A.B.", year: "2019", price:"55.00"},
          {image: "https://s.lubimyczytac.pl/upload/books/4936000/4936081/934525-352x500.jpg", title: "Kajś. Opowieść o Górnym Śląsku", author: "Zbigniew Rokita", description: "Przez większość życia uważałem Ślązaków za jaskiniowców z kilofem i roladą. Swoją śląskość wypierałem. W podstawówce pani Chmiel grała nam na akordeonie Rotę, a ja nie miałem pojęcia, że ów plujący w twarz Niemiec z pieśni był moim przodkiem.", publisher: "Czarne", year: "2020", price:"32.00"},
          {image: "https://s.lubimyczytac.pl/upload/books/4963000/4963549/920062-352x500.jpg", title: "Święto ognia", author: "Jakub Małecki", description: "Tata tańczy za szybą. Podnosi rozmazane przez deszcz ręce i kołysze rozmazaną przez deszcz głową, ma zamknięte oczy i ja w końcu też zamykam swoje, i śpię, i budzę się rano, i znowu jesteśmy razem.", publisher: "Sine Qua Non", year: "2021", price:"41.00"},
          {image: "https://s.lubimyczytac.pl/upload/books/4882000/4882986/866945-352x500.jpg", title: "27 śmierci Toby’ego Obeda", author: "Joanna Gierak-Onoszko", description: "To też jest Kanada: siedem zapałek w słoiku, sny o czubkach drzew, powiewające na wietrze czerwone suknie, dzieci odbierane rodzicom o świcie. I ludzie, którzy nie mówią, że są absolwentami szkół z internatem. Mówią: jesteśmy ocaleńcami. Przetrwaliśmy.", publisher: "Dowody na Istnienie", year: "2019", price:"56.00"},
        ],
        priceFilter:{
          valueFrom: '',
          valueTo: '',
        },
        bookItem:{
          title:'',
          author:'',
          description:'',
          publisher:'',
          year:'',
          price:'',
          image:'',
          inputRules: [
            v => !!v || 'This field can not be empty'
          ],
          urlRules: [
            v => !!v || 'This field can not be empty',
            v => this.isURL(v) || 'URL is not valid',
          ],
          priceRule: [
              v => !!v || 'This field can not be empty',
              v => (!isNaN(parseFloat(v)) && v >= 0 && v <= 5000) || 'Price has to be between 0 and 5000'
          ]
        },
        dialogNew: false,
        dialogEdit: false
      }
    },
    methods: {
      sortBy(prop) {
        this.booksRepository.sort((a,b) => a[prop] < b[prop] ? -1 : 1)
        this.books = this.booksRepository
      },
      isNumber: function(evt) {
        evt = (evt) ? evt : window.getEvents;
        var charCode = (evt.which) ? evt.which : evt.keyCode;
        if ((charCode > 31 && (charCode < 48 || charCode > 57)) && charCode !== 46) {
          evt.preventDefault();
        } else {
          return true;
        }
      },
      submit() {
        if(this.$refs.form.validate()){
          const book = {
            image: this.bookItem.image,
            title: this.bookItem.title,
            author: this.bookItem.author,
            description: this.bookItem.description,
            publisher: this.bookItem.publisher,
            year: this.bookItem.year,
            price: this.bookItem.price,
          }
          this.booksRepository.push(book)
          this.books = this.booksRepository
          this.dialogNew = false
        }
      },
      removeItem(index) {
        this.booksRepository.splice(index, 1)
        this.books = this.booksRepository
      },
      isURL(str) {
        let url;

        try {
          url = new URL(str);
        } catch (_) {
          return false;
        }

        return url.protocol === "http:" || url.protocol === "https:";
      },
      filterByPrice() {
        this.resetFilters()
        let from = this.priceFilter.valueFrom;
        let to = this.priceFilter.valueTo;
        if (from == null || from === "" || to == null || to === "") {
          alert("To filter by price enter valid min&max prices");
          return
        }
        console.log(from>to)
        if (parseInt(from) > parseInt(to)) {
          alert("Form value is bigger than To. Please correct filter");
          return
        }
        function getFilteredBooks(book) {
          return (book['price'] >= from && book['price'] <= to)
        }
        this.books = this.books.filter(getFilteredBooks)
      },
      resetFilters() {
        this.books = this.booksRepository
      },
      provideBookData(index) {
        this.dialogEdit = true
        const bookToEdit = this.books.at(index)
        this.bookItem.title = bookToEdit.title
        this.bookItem.author = bookToEdit.author
        this.bookItem.description = bookToEdit.description
        this.bookItem.publisher = bookToEdit.publisher
        this.bookItem.year = bookToEdit.year
        this.bookItem.price = bookToEdit.price
        this.bookItem.image = bookToEdit.image
      },
      updateUser(index) {
        console.log(index)
        this.booksRepository.splice(index, 1, {
          image: this.bookItem.image,
          title: this.bookItem.title,
          author: this.bookItem.author,
          description: this.bookItem.description,
          publisher: this.bookItem.publisher,
          year: this.bookItem.year,
          price: this.bookItem.price,
        })
          this.books = this.booksRepository
          this.dialogEdit = false
      }
    }
  }
</script>

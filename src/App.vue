<template>
  <div id="app">
     <b-container class="mt-4">
       <b-row>

        <b-col lg="6">
          <label for="textarea1">
            <h2>Шифр Цезаря</h2>
          </label>
          <b-form-textarea
            id="textarea1"
            size="lg"
            rows="6"
            placeholder="Напишите текст чтобы зашифровать его"
            v-model="text"
          ></b-form-textarea>
        </b-col>

        <b-col lg="6">
          <label for="textarea1">
            <h2>Результат</h2>
          </label>
          <b-form-textarea
            id="textarea2"
            size="lg"
            rows="6"
            disabled
            v-model="textCipher"
          ></b-form-textarea>
        </b-col>

      </b-row>

      <b-row class="mt-4">

        <b-col lg="2">
          <b-form-select v-model="selected"
                         :options="options">
          </b-form-select>
          <h4 class="mt-3">Сдвиг: <strong>{{ selected }}</strong></h4>
        </b-col>

        <b-col lg="2" sm="3">
          <b-button @click="encrypt" class="btn-encrypt"
                    variant="primary">
            <b-icon-lock-fill /> Зашифровать
          </b-button>
        </b-col>

        <b-col lg="2" sm="4">
          <b-button @click="decrypt" class="btn-decrypt"
                    variant="success">
            <b-icon-unlock-fill /> Расшифровать
          </b-button>
        </b-col>

      </b-row>

     </b-container>
  </div>
</template>

<script>
import 'bootstrap/dist/css/bootstrap.css'
import 'bootstrap-vue/dist/bootstrap-vue.css'

export default {
  name: 'App',
  data() {
    return {
      selected: null,
      text: '',
      textCipher: '',
      alphabet: 'АБВГДЕЁЖЗИЙКЛМНОПРСТУФХЦЧШЩЪЫЬЭЮЯ',
      alphabetCipher: ''
    }
  },
  computed: {
    options() {
      let options = [ { value: null, text: 'Выбрать' } ]

      for (let i = 1; i <= 33; i++) {
        options.push({ value: i, text: i })
      }

      return options

    }
  },
  methods: {
    encrypt() {
      this.alphabetCipher = this.alphabet
      this.alphabetCipher = this.shiftArray( this.alphabetCipher.split(''), this.selected ).join('')

      this.textCipher = this.cryptText( this.text, this.alphabet, this.alphabetCipher )
    },
    decrypt() {
      this.alphabetCipher = this.alphabet
      this.alphabetCipher = this.shiftArray( this.alphabetCipher.split(''), this.selected ).join('')

      this.textCipher = this.cryptText( this.text, this.alphabetCipher, this.alphabet )
    },
    shiftArray(arr, num) {
      return arr.slice(num).concat( arr.slice(0, num) )
    },
    cryptText(text, alphabet, alphabetCipher) {
      const numArr = []
      const textArr = text.split('')

      // добавляем в алфавит и нижний его регистр
      alphabet = alphabet + alphabet.toLowerCase()
      alphabetCipher = alphabetCipher + alphabetCipher.toLowerCase()

      // проверка есть ли в ориг.алфавите такая буква
      textArr.forEach(el => {
        if ( alphabet.indexOf(el) !== -1 ) {
           numArr.push( alphabet.indexOf(el) )
        } else {
           numArr.push(el)
        }
      })

      // заменяем на букву из криптованного алфавита
      return numArr.map(n => {
        let arr = []

        // иначе если это символ, оставить как есть
        if ( Number.isInteger(n) ) {
          arr = alphabetCipher[n]
        } else {
          arr = n
        }

        return arr

      }).join('')

    }
  }
}
</script>

<style>
body {
  background-color: #f9733f;
  font-family: 'Roboto', 'Arial', sans-serif;
}

.btn-encrypt,
.btn-decrypt {
  min-width: 163px;
}

@media screen and (max-width: 1200px) {
    .btn-decrypt {
      margin-left: 10px;
    }
}

@media screen and (max-width: 580px) {
    .btn-decrypt {
      margin: 20px 0;
    }

    ::placeholder {
      font-size: 16px;
    }

    h2 {
      font-size: 24px;
    }

    #textarea1 {
      margin-bottom: 20px;
    }
}
</style>

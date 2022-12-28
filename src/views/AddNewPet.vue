<template>
  <html lang="en">
  <AlertError :message="errorResponse.message"/>
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    <meta name="description" content="">
    <meta name="author" content="">

    <title>Add a new pet</title>

    <!-- Bootstrap core CSS -->
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/css/bootstrap.min.css"
          integrity="sha384-Gn5384xqQ1aoWXA+058RXPxPg6fy4IWvTNh0E263XmFcJlSAwiGgFAW/dAiS6JXm" crossorigin="anonymous">
  </head>

  <body class="bg-light">

  <div class="container">
    <div class="py-5 text-center">
      <h2>Add a new pet</h2>
    </div>

    <div class="row">
      <div class="col-md-12 order-md-1">
        <form class="needs-validation" novalidate="">

          <div class="mb-3">
            <label for="name">Name</label>
            <div class="input-group">
              <input v-model="addPetRequest.name" type="text" class="form-control" id="name" placeholder="Name">
            </div>
          </div>

          <div class="mb-3">
            <label for="code">Code</label>
            <div class="input-group">
              <input v-model="addPetRequest.code" type="text" class="form-control" id="code" placeholder="Code">
            </div>
          </div>

          <div class="mb-3">
            <label for="types">Type</label>
            <select v-model="addPetRequest.typeId" class="custom-select d-block w-100" id="types">
              <option selected disabled value="0">--type--</option>
              <option v-for="type in typesDropdownInputs" :key="type.typeId" :value="type.typeId">{{ type.typeName }}</option>
            </select>
          </div>

          <div class="mb-3">
            <label for="colors">Fur color</label>
            <select v-model="addPetRequest.colorId" class="custom-select d-block w-100" id="colors">
              <option selected disabled value="0">--color--</option>
              <option v-for="color in colorsDropdownInputs" :key="color.colorId" :value="color.colorId">{{ color.furColor }}</option>

            </select>
          </div>

          <div class="mb-3">
            <label for="countries">Country of origin</label>
            <select v-model="addPetRequest.countryId" class="custom-select d-block w-100" id="countries">
              <option selected disabled value="0">--country--</option>
              <option v-for="country in countriesDropdownInputs" :key="country.countryId" :value="country.countryId">{{country.countryName }}
              </option>

            </select>
          </div>

          <button v-on:click="createNewPet" class="btn btn-primary btn-lg btn-block" type="submit">
            Add Pet
          </button>
        </form>
      </div>
    </div>

    <footer class="my-5 pt-5 text-muted text-center text-small">
      <p class="mb-1">© 2022 Wisercat</p>
    </footer>
  </div>


  </body>
  </html>
</template>

<script>
import AlertError from "@/components/alert/AlertError";

export default {
  name: "AddNewPet",
  components: {AlertError},
  data: function () {
    return {
      colorsDropdownInputs: [
        {
          colorId: 0,
          furColor: ''
        }
      ],
      typesDropdownInputs: [
        {
          typeId: 0,
          typeName: ''
        }
      ],
      countriesDropdownInputs: [
        {
          countryId: 0,
          countryName: ''
        }
      ],
      addPetRequest: {
            userId: sessionStorage.getItem('userId'),
            name: "",
            code: "",
            colorId: 0,
            countryId: 0,
            typeId: 0
          },
      addPetResponse: {
        name: "",
        code: "",
        colorId: 0,
        countryId: 0,
        typeId: 0
      },
      errorResponse: {
        message: '',
        errorCode: 0
      }
    }
  },

  methods: {

    createNewPet: function () {
      this.errorResponse.message = ''
      if (this.addPetRequest.name.length == 0 || this.addPetRequest.code.length == 0) {
        this.displayRequiredFieldsNotFilledAlert();
      } else {
        this.addNewPet()
      }
    },

    addNewPet: function () {
      this.$http.post("/add", this.addPetRequest
      ).then(response => {
        this.addPetResponse = response.data
        this.navigateHome()
      }).catch(error => {
        console.log(error)
      })
    },

    navigateHome: function () {
      // sessionStorage.setItem('petId', this.addPetResponse.petId)
      this.$router.push({
        name: 'home'
      })
    },

    displayRequiredFieldsNotFilledAlert: function () {
      this.errorResponse.message = 'Fill all inputs'
    },

    getColorsDropdown: function () {
      this.$http.get("/colors")
          .then(response => {
            this.colorsDropdownInputs = response.data
          })
          .catch(error => {
            console.log(error)
          })
    },

    getCountriesDropdown: function () {
      this.$http.get("/countries")
          .then(response => {
            this.countriesDropdownInputs = response.data
          })
          .catch(error => {
            console.log(error)
          })
    },
    getTypesDropdown: function () {
      this.$http.get("/types")
          .then(response => {
            this.typesDropdownInputs = response.data
          })
          .catch(error => {
            console.log(error)
          })
    }
  },
  beforeMount() {
    this.getColorsDropdown()
    this.getCountriesDropdown()
    this.getTypesDropdown()
  }
}
</script>

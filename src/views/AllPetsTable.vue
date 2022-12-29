<template>
  <div>
    <head>
      <meta charset="utf-8">
      <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
      <meta name="description" content="">
      <meta name="author" content="">

      <title>Pets list</title>

      <!-- Bootstrap core CSS -->
      <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/css/bootstrap.min.css"
            integrity="sha384-Gn5384xqQ1aoWXA+058RXPxPg6fy4IWvTNh0E263XmFcJlSAwiGgFAW/dAiS6JXm" crossorigin="anonymous">

    </head>
    <div class="container">
      <div class="py-5 text-center">
        <h2>Pets</h2>
      </div>

      <div class="row">
        <button v-on:click="navigateToAddNewPet" class="btn btn-primary">Add pet</button>
        <table class="table">
          <thead>
          <tr>
            <th scope="col">#</th>
            <th scope="col">Name</th>
            <th scope="col">Code</th>
            <th scope="col">Type</th>
            <th scope="col">Fur color</th>
            <th scope="col">Country</th>
            <th></th>
          </tr>
          </thead>
          <tbody>
          <tr v-for="pet in pets">
            <th scope="row">{{ pet.petId }}</th>
            <td>{{ pet.petName }}</td>
            <td>{{ pet.petCode }}</td>
            <td>{{ pet.typeName }}</td>
            <td>{{ pet.colorColor }}</td>
            <td>{{ pet.countryName }}</td>
            <td>
              <button v-on:click="editPet(pet.petId)" class="btn btn-primary">Edit pet</button>
            </td>
          </tr>


          </tbody>
        </table>
      </div>
    </div>

  </div>
</template>
<script>
export default {
  name: 'AllPetsTable',
  data: function () {
    return {
      pets: [
        {
          petId: 0,
          petName: '',
          petCode: '',
          typeName: '',
          colorColor: '',
          countryName: ''
        }
      ]
    }
  },
  methods: {
    getAllPets: function () {
      this.$http.get("/pets")
          .then(response => {
            this.pets = response.data
          })
          .catch(error => {
            console.log(error)
          })
    },

    navigateToAddNewPet: function () {
      this.$router.push({
        name: 'addNewPet'
      })
    },

    editPet: function (petId) {
      sessionStorage.setItem('petId', petId)
      this.$router.push({
        name: 'editPet'
      })
    }
  },

  beforeMount() {
    this.getAllPets()
  }

}
</script>
<template>
  <div id="app" class="container">
      <h1>Twister</h1>
      <input @click="actionNext" type="button" class="btn btn-success btn-block go" :value="btnValue">
      <div v-if="action" class="action">
         <p>{{ action }}</p>
      </div>
    <div class="row formu">
        <div class="col-md-9">
            <form>
            <div class="input-group">
                    <input class="form-control" v-model="nom" type="text" placeholder="Nom du joueur">
                    <span class="input-group-btn">
                        <button class="btn btn-info" type="submit" @click.prevent="addName">Ajouter</button>
                    </span>
            </div>
            </form>
        </div>
    </div>
    <div>
      <div class="panel panel-default">
          <div class="panel-heading">
              <h3 class="panel-title">Joueurs</h3>
          </div>
          <div class="panel-body">
              <ul class="list-group">
                  <li class="list-group-item" v-for="(name, index) in names" :key="index">
                      {{name}} <button :prop=index type="button" class="close" @click="removeName(index)"><span aria-hidden="true">&times;</span></button>
                  </li>
              </ul>
          </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
        names: [],
        couleurs: ['bleu', 'rouge', 'jaune', 'vert'],
        membre: ['bras droit', 'bras gauche', 'pied droit', 'pied gauche'],
        nom: '',
        action: 'Ready ?',
        joueurActif: 0,
        btnValue: 'Go !'
    }
  },
  created () {
    var users = localStorage.getItem('joueursTableau')
    if (users){
        var usersArray = users.split(',')
        console.log('Joueurs : ' + usersArray)
        for (var i = 0; i < usersArray.length; i++){
            this.names[i] = usersArray[i]
        }
    }else {
        console.log('Aucun joueur')
    }
  },
  methods: {
    addName() {
        if (this.nom != '') {
            this.names.push(this.nom)
            this.nom = ''
            localStorage.setItem('joueursTableau', this.names)
        }
    },
    actionNext () {

            // Choix du joueur
            var nbJoueur = this.names.length
            if (this.joueurActif > (nbJoueur - 1)){
                this.joueurActif = 0
            }
            this.action = this.names[this.joueurActif]
            this.joueurActif++

        if (this.action != undefined ){


            // Choix du membre
            var nbMembre = this.membre.length
            var valRandomMembre = Math.floor(Math.random() * nbMembre)
            this.action += ', ' + this.membre[valRandomMembre] + ' sur le'

            // Choix de la couleur
            var nbCouleur = this.couleurs.length
            var valRandomCouleur = Math.floor(Math.random() * nbCouleur)
            this.action += ' ' + this.couleurs[valRandomCouleur]

            this.btnValue = 'Suivant !'
        }else {
            this.action = 'Vous devez ajouter des joueurs !'
        }
    },
    removeName (valeur) {
        this.names.splice(valeur,1)
    }
  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
    .formu {
        margin-top: 30px;
    }
    .go {
        height: 60px;
    }
    .action {
        height: 80px;
        display: flex;

        p {
            margin: auto;
            font-size: 1.8em;
        }
    }
</style>

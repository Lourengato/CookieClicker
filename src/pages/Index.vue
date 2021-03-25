<template>
  <q-page>
    <div>Poeira Espacial: {{game.spaceDust}}</div>
    <div>Poeira espacial por segundo: {{game.multiply}}</div>
    <div>Poeira espacial por clique: {{game.cursorMultiply}}</div>

    <q-btn color="pink" label="clique aqui" @click="addSpacedust" />

    <q-card @click="add('spaceship')">
      <div>Espaçonave</div>
      <div>preço: {{game.spaceship.price}}</div>
      <div>quantidade: {{game.spaceship.quantity}}</div>
    </q-card>

    <q-card @click="add('cursor')">
      <div>Cursor</div>
      <div>preço: {{game.cursor.price}}</div>
      <div>quantidade: {{game.cursor.quantity}}</div>
    </q-card>
  </q-page>
</template>

<script>
export default {
  data () {
    return {
      game: {
        spaceDust: 0,
        multiply: 1,
        cursorMultiply: 1,
        spaceship: {
          price: 5,
          priceMultiply: 1.5,
          quantity: 0,
          itemMultiply: 2,
        },
        cursor: {
          price: 5,
          priceMultiply: 1.5,
          quantity: 0,
          itemMultiply: 2,
        },
      },
      
    }
  },

  mounted () {
    this.spaceTimer()

    const gameSave = localStorage.getItem('game')
    if (gameSave) {
      try {
        this.game = JSON.parse(gameSave)
      } catch {
        localStorage.removeItem('game')
      }
    }
  },

  methods: {
    spaceTimer () {
      setTimeout(() => {
        this.game.spaceDust = Math.floor(this.game.spaceDust + this.game.multiply)
        this.spaceTimer()
        this.saveGame()
      }, 1000)
    },

    saveGame () {
      const game = JSON.stringify(this.game)
      localStorage.setItem('game', game)
    },

    addSpacedust () {
      this.spaceDust = Math.floor(this.game.spaceDust + this.game.cursorMultiply)
    },

    add (item) {
      if (this.game.spaceDust >= this.game[item].price) {
        this.game.spaceDust -= this.game[item].price
        this.game[item].quantity++
        this.game[item].price = Math.floor(this.game[item].price * this.game[item].priceMultiply)
        if (item === 'cursor') {
          this.game.cursorMultiply += this.game[item].itemMultiply
          return
        }
        this.game.multiply += this.game[item].itemMultiply
      }
    }
  }
}
</script>

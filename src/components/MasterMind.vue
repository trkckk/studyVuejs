<template>
  <div class="container">

    <div class="card">

      <div class="card-header">
        <h1 class="card-title">Game Console</h1>
      </div>

      <div class="card-body">
        <div class="form-group">
          <h4>Level:<span class="badge text-bg-success">{{game.level}}</span></h4>
          <h4>Tries:<span class="badge text-bg-danger">{{game.tries}}</span> of <span class="badge bg-warning"></span>{{game.maxTriesTries}}</h4>
          <h4>Lives:<span class="badge text-bg-info">{{game.lives}}</span></h4>

        </div>

        <div class="form-group">
          <label class="form-label" for="guess">Guess:</label>
          <input type="text" class="form-control" v-model="game.quess">
         <div><button class="btn btn-success"
                      @click="play">Play</button></div>
        </div>

        <div class="form-group">
         <table class="table table-bordered table-hover table-striped">
           <thead>
            <tr>
              <th>No</th>
              <th>Guess</th>
              <th>Evaluation</th>
            </tr>
           </thead>
           <tbody>
              <th>1</th>
              <th>123</th>
              <th>-1 +1</th>
           </tbody>
         </table>
        </div>

      </div>

    </div>

  </div>
</template>

<script>
export default {
    name: 'MasterMind',
    props: {},
    methods:{
      play(){
          this.game.tries++;
      }
    },

  data: function (){
    return {

      game:{

          secret: 123,
          quess: 456,
          level: 3,
          tries: 0,
          lives: 3,
          maxTriesTries: 10,
          moves:[]
      },
      statistics:{

          wins: 0,
          loses:0
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>

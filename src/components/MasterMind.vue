<template>
    <p></p>
    <div class="container">

        <div class="card">

            <div class="card-header">
                <h1 class="card-title">Game Console</h1>
            </div>

            <div class="card-body">
                <div class="form-group">
                    <h4>Wins:<span class="badge bg-primary" style="color:red">{{statistics.wins}}</span>of<span class="badge bg-primary" style="color:#ff0000">{{total}}</span> </h4>
                    <h4>Loses:<span class="badge bg-success" style="color:red">{{statistics.loses}}</span>of<span class="badge bg-primary" style="color:red">{{total}}</span> </h4>
                    <h4>Level:<span class="badge bg-success" style="color:blue">{{game.level}}</span></h4>
                    <h4>Tries:<span class="badge bg-danger" style="color:green">{{game.tries}}</span> <span
                            class="table-striped" style="color: green">of</span><span class="badge bg-warning"></span>{{game.maxTriesTries}}
                    </h4>
                    <h4>Lives:<span class="badge bg-info" style="color:green">{{game.lives}}</span></h4>

                </div>

                <div class="form-group">
                    <label class="form-label" for="guess">Guess:</label>
                    <button class="btn btn-success"
                            @click="play">Play
                    </button>
                    <input type="text" class="form-control" v-model="game.guess">



                </div>

                <div class="form-group">
                    <table class="table table-bordered table-hover table-striped">
                        <thead>
                        <tr>
                            <th>No-</th>
                            <th>Guess-</th>
                            <th>Evaluation-</th>


                        </tr>
                        </thead>
                        <tbody>
                        <tr v-for="move in game.moves" :key="move.guess">
                            <th></th>
                            <th>{{move.guess}}</th>
                            <th>{{move.evaluation}}</th>

                        </tr>
                        </tbody>
                    </table>
                </div>

            </div>

        </div>

    </div>
</template>

<script>
    import Move from "@/model/move";

    export default {
        name: 'MasterMind',
        props: {},
        mounted(){
          this.game.secret = this.createSecret();
        },
        methods: {
            evaluateMove() {
                let guessAsString = 0;
                guessAsString =this.game.guess.toString();
                let secretAsString = 0;
                secretAsString =this.game.secret.toString();
                let perfectMatch = 0, partialMatch = 0;
                for (let i = 0; i < guessAsString.length; ++i) {
                    let g = guessAsString.charAt(i);
                    for (let j = 0; j < secretAsString.length; ++j) {
                        let s = secretAsString.charAt(j);
                        if (g === s) {
                            if (i === j) {
                                perfectMatch++;
                            } else {
                                partialMatch++;
                            }
                        }
                    }
                }

                return new Move(this.game.guess, perfectMatch, partialMatch);

            },
            play() {
                this.game.tries++;
                if(Number(this.game.secret) === Number(this.game.guess) ){
                    this.game.maxTriesTries += 2;
                    this.game.lives++;
                    this.game.level++;
                    this.statistics.wins++;
                    this.initGame();
                }else{

                    if(this.game.tries>= this.game.maxTriesTries){
                        this.game.lives--;
                        this.statistics.loses++;
                        if(this.game.lives === 0){
                            //TODO: player loses

                        }else{
                            this.initGame();
                        }
                    }else{
                        this.game.moves.push(this.evaluateMove());


                    }
                }
            },
            getRandomDigit(min, max) {
                return Math.floor(Math.random() * (max - min + 1)) + min;
            },

        initGame(){
                this.game.tries = 0;
                this.game.secret = this.createSecret();
                this.game.moves = [];

        },
        createSecret() {
            let digits = [];
            digits.push(this.getRandomDigit(1, 9));
            while (digits.length < this.game.level) {

                let digit = this.getRandomDigit(1, 9);
                if (digits.includes(digits)) continue;
                digits.push(digit);
            }
            console.log(digits);
            return digits.reduce((s, d) => 10 * s + d, 0);
        }
        },
        computed:{
            total(){
                return this.statistics.wins + this.statistics.loses;
            }
        }
    ,

    data: function () {
        return {

            game: {

                secret: 123,
                quess: 456,
                level: 3,
                tries: 0,
                lives: 3,
                maxTriesTries: 10,
                moves: []
            },
            statistics: {

                wins: 0,
                loses: 0
            }
        }
    }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>

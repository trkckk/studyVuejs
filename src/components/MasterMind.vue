<template>
    <p></p>
    <div class="container">
        <BootstrapModalDialog :dialog-text="`You have already used the guess (${this.game.guess})!`"
                              :close-dialog="closeDialog"
                              :visible="isDialogVisible"
                              dialog-title="Alert"></BootstrapModalDialog>
        <BootstrapCard>
            <BootstrapCardHeader header="Game Console"/>
            <BootstrapCardBody>
                <div class="mb-1">
                    <BootstrapLabel value="Wins:"/>
                    <BootstrapBadge color="badge bg-primary" :value="statistics.wins"/>
                    of
                    <BootstrapBadge color="bg-success" :value="total"/>
                </div>
                <div class="mb-1">
                    <BootstrapLabel value="Loses:"/>
                    <BootstrapBadge color="bg-secondary" :value="statistics.loses"/>
                    of
                    <BootstrapBadge color="bg-success" :value="total"/>
                </div>
                <div class="mb-1">
                    <BootstrapLabel value="Game Level:"/>
                    <BootstrapBadge color="bg-success" :value="game.level"/>
                </div>
                <div class="mb-1" v-if="game.tries === 0">
                    <BootstrapBadge color="bg-info" value="NEW GAME"/>
                </div>
                <div class="mb-1" v-if="game.tries > 0">
                    <BootstrapLabel value="Tries:"/>
                    <BootstrapBadge color="bg-danger" :value="game.tries"/>
                    of
                    <BootstrapBadge color="bg-warning" :value="game.maxTries"/>
                </div>
                <div class="mb-1">
                    <BootstrapProgressBar :value="game.counter" label="Time:" :color="progressBarColor"/>
                </div>
                <div class="mb-3">
                    <BootstrapLabel value="Lives:"/>
                    <BootstrapBadge color="badge bg-info" :value="game.lives"/>
                </div>
                <div class="mb-3">
                    <label class="form-label card-text mb-2" for="guess">Guess:</label>
                    <input type="text" class="form-control mb-2" v-model="game.guess">
                    <button class="btn btn-success" @click="play">Play</button>
                </div>
                <p></p>
                <div class="mb-3">
                    <BootstrapTable v-if="game.moves.length > 0">
                        <BootstrapTableHeader :headers="['Move No', 'Guess', 'Evaluation']"></BootstrapTableHeader>
                        <tbody>
                        <tr v-for="(move,index) in game.moves" :key="move.guess">
                            <td>{{ index + 1 }}</td>
                            <td>{{ move.guess }}</td>
                            <td>
                                <MasterMindEvaluation :move="move"></MasterMindEvaluation>
                            </td>
                        </tr>
                        </tbody>
                    </BootstrapTable>
                </div>
            </BootstrapCardBody>
        </BootstrapCard>
    </div>
</template>

<script>

    import BootstrapBadge from "@/components/BootstrapBadge";
    import BootstrapLabel from "@/components/BootstrapLabel";
    import BootstrapProgressBar from "@/components/BootstrapProgressBar";
    import BootstrapTable from "@/components/BootstrapTable";
    import BootstrapTableHeader from "@/components/BootstrapTableHeader";
    import BootstrapCardHeader from "@/components/BootstrapCardHeader";
    import MasterMindEvaluation from "@/components/MasterMindEvaluation";
    import BootstrapModalDialog from "@/components/BootstrapModalDialog";
    import BootstrapCard from "@/components/BootstrapCard";
    import BootstrapCardBody from "@/components/BootstrapCardBody";
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

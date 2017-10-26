<template>
    <div class="container">
        <!-- <pre>{{ rows }}</pre> -->
        <!-- <pre>{{ fogon }}</pre> -->
        <div
            v-for="(row,indexRow) in rows"
            class="row">

            <div
                v-for="(box, indexBox) in row"
                class="col-xs-3 col-lg-3 no-padding-right no-padding-left box" >
                <div class="number">
                    <template v-if="box.inUse">{{ box.number }}</template>
                </div>
            </div>
        </div>
    </div>
</template>


<script>
export default{
    created(){
        //Creating rows
        let rows = [];
        for (let i = 0; i < 4; i++) {
            rows[i] = [];
            //Creating boxes
            for (let j = 0; j < 4; j++) {
                rows[i][j] = Object.assign({}, this.boxBlank);
            }
        }
        //Set rows and boxes, using assign to make them reactive
        this.rows = Object.assign([], this.rows, rows);
        this.setNumber();
        this.setNumber();

        window.addEventListener('keyup', this.keyPressed);
    },
    methods:{
        getRandomNumber(){
            return Math.floor(Math.random() * (3 - 0 + 1)) + 0;
        },
        setNumber(){

            if(this.finished){
                return 'Game finished';
            }

            let randomSet = false;
            let x = -1;
            let y = -1;

            while(!randomSet){
                x = this.getRandomNumber();
                y = this.getRandomNumber();

                let box = this.rows[x][y];

                if(!box.inUse){
                    randomSet = true;
                    this.rows[x][y].inUse = true;
                    this.rows[x][y].number = (parseInt(Math.random() * 2) ?  true : false) ? 2 : 4;

                    console.log('Set number in: '+ x +','+y);
                }
            }
        },
        keyPressed(event){
            console.log('keyPressed');
            let move_allowed = false;
            let key_pressed = event.which;

            if(key_pressed >= 37 && key_pressed <= 40){

                for(let i = 0; i < 4 ; i++){
                    for(let j = 0 ; j < 4 ; j++){
                        this.doMovement(i,j);
                    }
                }

                if(this.canKeepPlaying()){
                    this.setNumber();
                }
            }
        },
        doMovement(x,y){
            var initialBox = this.rows[x][y];
            if(initialBox.inUse){
                console.log('doMovement '+x+','+y);
                var actualBox;
                var i;

                for(i = (y-1); i >= 0; i--){
                    console.log(x + ',' + i);

                    actualBox = this.rows[x][i];
                    initialBox = this.rows[x][i+1];
                    console.log(actualBox);

                    if(typeof actualBox == 'undefined'){
                        break;
                    }

                    if(actualBox.inUse){
                        console.log('inUse');
                        if(actualBox.number == initialBox.number){
                            console.log('igualayas');
                            actualBox.number *= 2;
                            initialBox.number = 0;
                            initialBox.inUse = false;
                        }
                    }else{
                        console.log('notInUse');

                        actualBox.inUse = true;
                        actualBox.number = initialBox.number;
                        initialBox.number = 0;
                        initialBox.inUse = false;
                    }
                }
            }
        },
        canKeepPlaying(){
            let keep_playing = false;

            looping:
            for(let i = 0; i < 4 ; i++){
                for(let j = 0 ; j < 4 ; j++){
                    if(!this.rows[i][j].inUse){
                        keep_playing = true;
                        break looping;
                    }
                }
            }
            return keep_playing;
        }
    },
    data(){
        return {
            fogon: 44,
            prueba :'holaa',
            finished:false,
            rows:[
            ],
            boxBlank:{
                inUse: false,
                number: 0
            },
        };
    }
}
</script>

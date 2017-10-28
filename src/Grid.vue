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

                switch(key_pressed){
                    case 37: key_pressed = 'left';break;
                    case 38: key_pressed = 'up';break;
                    case 39: key_pressed = 'right';break;
                    case 40: key_pressed = 'down';break;
                }
                console.log(key_pressed);


                for(let i = 0; i < 4 ; i++){
                    for(let j = 0 ; j < 4 ; j++){
                        this.doMovement(i,j, key_pressed);
                    }
                }

                if(this.canKeepPlaying()){
                    this.setNumber();
                }
            }
        },
        doMovement(x,y, key_pressed){
            var initialBox = this.rows[x][y];

            if(initialBox.inUse){
                console.log('doMovement '+x+','+y);
                var actualBox;
                var i;
                var symbol;
                var operation;
                var spinning = true;

                if(key_pressed == 'up' || key_pressed == 'left'){
                    symbol = '>=';
                    operation = '-';
                }else{
                    symbol = '<=';
                    operation = '+';
                }

                i  = key_pressed == 'up' || key_pressed ==  'down' ? x : y ;


                while(spinning){
                    console.log(operation);
                    i = operation == '-' ? i - 1 : i + 1 ;
                    spinning = symbol == '>=' ? i >= 0 : i <= 3;

                    if(!spinning){
                        break;
                    }


                    if(key_pressed == 'up' || key_pressed == 'down'){
                        actualBox = this.rows[i][y];
                        initialBox = key_pressed == 'up' ? this.rows[i+1][y] : this.rows[i-1][y] ;
                        console.log(initialBox);

                    }else{
                        actualBox = this.rows[x][i];
                        initialBox = key_pressed == 'left' ? this.rows[x][i+1] : this.rows[x][i-1];
                        console.log(x+','+i);

                    }

                    console.log(actualBox);

                    if(typeof actualBox == 'undefined'){
                        console.log('chau');
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

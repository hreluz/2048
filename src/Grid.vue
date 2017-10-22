<template>
    <div class="container">
        <div
            v-for="(row,index) in rows"
            class="row">

            <div
                v-for="(box, index) in row"
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
        for (let i = 0; i < 4; i++) {
            this.rows[i] = [];

            //Creating boxes
            for (let j = 0; j < 4; j++) {
                this.rows[i][j] = {};
                this.rows[i][j].inUse = false;
                this.rows[i][j].number = 0;
            }
        }

        this.setNumber();
        this.setNumber();
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
                console.log(x + ','+y );

                if(!this.rows[x][y].inUse){
                    randomSet = true;

                    this.rows[x][y].inUse = true;
                    this.rows[x][y].number = (parseInt(Math.random() * 2) ?  true : false) ? 2 : 4;
                    console.log(this.rows[x][y]);
                }
            }
        },
    },
    data(){
        return {
            finished:false,
            rows:[
            ]
        };
    }
}
</script>

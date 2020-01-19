<template>
    <div>
        <!-- Rows is : {{rows}} -->
        <div v-if="loading">
            Data is loading...
        </div>
        
        <div v-else>
            <div class="row mb-4" v-for="row in rows" :key="'row' + row">
                <div class="col d-flex align-items-stretch" v-for="(bookable, column) in bookablesInRow(row)" :key="'row' + row + column">
                    <bookable-list-item
                        v-bind="bookable"
                    >
                    </bookable-list-item>
                </div>
                <div class="col" v-for="p in placeHoldersInRow(row)" :key="'placeholder' +row +p"></div>
            </div>
 
        </div>
    </div>
</template>

<script>
import BookableListItem from "./BookableListItem"
export default {
    components: {
        BookableListItem
    },

    //components defined in data are reactive
    data: ()=>{
        return {
            bookables: null,
            loading: false,
            columns: 3
        };
    },

    computed: {
        rows(){
            return this.bookables == null ? 0 : Math.ceil(this.bookables.length / this.columns)
        }
    },
    // beforeCreate(){
    //     console.log('before create')
    // },

    methods: {
        bookablesInRow(row){
            return this.bookables.slice((row-1) * this.columns, row * this.columns)
        },

        placeHoldersInRow(row){
            return this.columns - this.bookablesInRow(row).length;
        }
    },

    //good place to fetch any data from the server
    created() {

        this.loading = true;

        // const p = new Promise((resolve,reject)=>{
        //     console.log(resolve);
        //     console.log(reject);

        //     setTimeout(()=>resolve("Hello there"), 300)
        // }).then((result) => console.log(`Success ${result}`))
        //     .catch((result) =>console.log(`Error ${result}`));

        // console.log(p);


        const request = axios.get("/api/bookables")
        .then(response => {
            this.bookables = response.data.data
            // this.bookables.push({title: "x", description : "Y"})
            this.loading = false
            });
        console.log(request);

        // setTimeout(()=> {
        //     this.bookables =[{
        //         title: "Cheap villa",
        //         content: "Cheap and affordable"
        //     },
        //     {
        //         title: "Cheap villa 2",
        //         content: "Cheap and affordable again"
        //     },
        //     {
        //         title: "Cheap villa",
        //         content: "Cheap and affordable"
        //     },
        //     {
        //         title: "Cheap villa 2",
        //         content: "Cheap and affordable again"
        //     },
        //     {
        //         title: "Cheap villa",
        //         content: "Cheap and affordable"
        //     },
        //     {
        //         title: "Cheap villa 2",
        //         content: "Cheap and affordable again"
        //     },
        //     {
        //         title: "Cheap villa",
        //         content: "Cheap and affordable"
        //     },
        //     {
        //         title: "Cheap villa 2",
        //         content: "Cheap and affordable again"
        //     }];
        //     this.loading = false;
        // }, 3000);

        // setTimeout(()=>{
        //     console.log('first change')
        //     this.bookable1.title = "Now you see me"
        // }, 8000);

        // setTimeout(()=>{
        //     console.log("second change")
        //     this.bookable3.title = "Now you don't see me"
        // }, 12000)
    },
    // beforeMount() {
    //     console.log('before mount')
    // },

    // mounted() {
    //     console.log('mounted')
    // },
    // beforeDestroy() {
    //     console.log('before destroy')
    // },
    // destroyed() {
    //     console.log('destroyed')
    // },
}
</script>
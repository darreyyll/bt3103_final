<template>
    <div>
        <navi></navi>
        <br><br>
        <h1> Zoning </h1>
        <h3>Zoning Policies</h3>
        <h3> Filter By:
            <select v-model="selectedoption" @change="get_policies()">
                <option value="All Policies" > All Policies </option>
                <option value="Campus Policies">Campus Policies</option>
                <option value="Visitor Policies">Visitor Policies</option>
                <option value="Student Life and CCA Activities">Student Life and CCA Activities</option>
                <option value="Residential Policies">Residential Policies</option>
                <option value="Other Policies">Other Policies</option>
            </select></h3>

            <br>

        <div class="notice_cont">
            <h2 style="text-align:center"> <u>{{selectedoption}} </u> </h2>
            <ul>
                    <li v-for="fb in policies" v-bind:key="fb.category" @click="toggleShow">
                        <div class="category">  <p><u>{{fb.title}} </u> </p>
                        <div class="details"> <p> <i> {{fb.category}}, posted on {{fb.date}} </i> </p> </div>
                        </div>
                        <div class="contents" v-if="showcontents"> <p>{{fb.content}}</p> </div>
                        
                    </li>
                </ul>
        </div>
        <div>
        <button id=back v-on:click="redirectToZoning()"> Back </button>
        </div>

        <br>

    </div>
</template>

<script>
import database from '../firebase.js'

export default {
    data() {
        return {
            selectedoption: "",
            policies: [],
            showcontents : ""
        }
    },

    methods: {
        redirectToZoning() {
            this.$router.push({path: '/zoning'})
        },
        get_policies : function() {
            this.policies = []
            database.collection("circulars").get().then((queryDocumentSnapShot) => {           
                    let fb = {}         
                    queryDocumentSnapShot.forEach(doc => { 
                        if(doc.data().category == this.selectedoption) {
                            fb = doc.data()
                            console.log(fb)
                            this.policies.push(fb)
                        } 
                        if(this.selectedoption == "All Policies") {
                            fb = doc.data()
                            this.policies.push(fb)
                        }
                        
                        
                    })
                }
            )
        },
        toggleShow : function() {
            if(this.showcontents) {
                this.showcontents = false;
            } else {
                this.showcontents = true;
            }

        }
    },
    created : function() {
        this.get_policies()
    }

    

}
</script>


<style scoped>
button {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    font-size: 16px;
    margin: 20px;
    text-align:'center';
    width: 100px;
    height: 40px;
    background: orange;
    color: white;
    background-color: #EF7C00;
    border-radius: 4px;
    float: center;
    
}

button:hover {
    background-color: orangered;
}

#back {
    right: 0;
    bottom: 0;
    position: absolute;
    background-color: #003D7C;

}

.notice_cont {
    width: 80%;
    height: 450px;
    position: relative;
    margin: auto;
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    letter-spacing: 0.75px;
    text-decoration: none;
    padding: 20px;
    background: lightgrey;
    border: none;
    cursor: pointer;
    text-align: left;
    overflow: auto;
    }   

    .category {
        font-size: 1.25em;
        text-align: left;
        cursor: pointer;
        border:none;
    }

    .details {
        color: gray;
        text-align: left;
        font-size: smaller;
    }

    .contents {
        font-size: 1em;
        text-decoration: none;
        text-align: left;
    }

</style>

        

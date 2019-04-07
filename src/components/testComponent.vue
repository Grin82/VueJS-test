<template>
    <div>
        <b-tabs pills card content-class="mt-3" class="widget forms">
            <b-tab :title-link-class=" [tab.tabColor, tab.tabText]"  v-for="tab in tabs" :key="`dyn-tab-${tab.id}`">
                <template slot="title">
                    <div>{{ title(tab.title, tab.quantity, tab.price) }}</div>
                </template>
                <div class="textDiv">
                    <div class=text>Enter title </div>
                    <div class="inputField"><input type="text" v-model="tab.title" required></div>
                </div>
                <div v-for="service in tab.services" :key="service.name" class="buttons">
                    <b-button size="sm" :variant="service.color" @click="tab.tabColor = service.tabColor;tab.tabText = service.tabText; tab.service = service.name;">
                        {{service.name}}
                    </b-button>
                </div>
                <div class="textDiv">
                    <div class=text>Enter quantity </div>
                    <div class="inputField"><input type="number" v-model="tab.quantity" required></div>
                </div>
                <div class="textDiv">
                   <div class=text>Enter price </div>
                   <div class="inputField"><input type="number" v-model="tab.price" required></div>
                </div>
                <div class="total">Total price: {{ totalCost(tab.quantity,tab.price) }}</div>
                <b-button size="sm" variant="success" class="float-center" @click.prevent="addTab">
                    ADD
                </b-button>
                <b-button size="sm" variant="dark" class="float-center bottomButtons" @click.prevent="removeTab(tab.id)">
                    REMOVE
                </b-button>
                <b-button size="sm" variant="secondary" class="float-center" @click.prevent="cloneTab(tab.id)">
                    CLONE
                </b-button>
            </b-tab>
        </b-tabs>
        <div class="submit">
            <button class="forms" @click.prevent="submitTabs(tabs)">Submit</button>
        </div>
    </div>
</template>

<script>

export default {
    name: 'testComponent',
    data:function(){
        return {
            col:{tabColor:'bg-primary',tebText:'text-light'},
            tabResult:null,
            allTabs:[],
            jsonTabs:"",
            base:{
                id:null,
                title: "",
                amount: "",
                service: "",
                tabColor: 'bg-primary',
                tabText: 'text-light',            
                services:[
                    {
                        name: 'Service 1',
                        color: 'info',
                        tabColor: 'bg-info',
                        tabText: 'text-light'
                    },
                    {
                        name: 'Service 2',
                        color: 'danger',
                        tabColor: 'bg-danger',
                        tabText: 'text-light'
                    },
                    {
                        name: 'Service 3',
                        color: 'warning',
                        tabColor: 'bg-warning',
                        tabText: 'text-light'
                    },                    
                ],
                quantity: "",
                price: "",
                totalCost: "",   
            },
            tabs:[]
        }
    },
    methods:{
        totalCost: function(price, quantity){
            return price * quantity;
        },
        title:function(title, price, quantity){
            
            if((this.amount = this.totalCost(price, quantity) > 0))
            {
                title = title + ' - ' + this.totalCost(price, quantity);
            }
            return title;
        },
        addTab:function(){
            var newItem = Object.assign({}, this.base);
            newItem.id = this.tabs.length;
            this.$set(this.tabs,this.tabs.length,newItem)
        },
        removeTab:function(id){
            for (var i = 0; i < this.tabs.length; i++) {
                if (this.tabs[i].id === id) {
                    this.tabs.splice(i, 1)
                }
            }
        },
        cloneTab:function(id){
            for (var i = 0; i < this.tabs.length; i++) {
                if (this.tabs[i].id === id) {
                    var newItem =  Object.assign({}, this.tabs[i]);
                    newItem.id = this.tabs.length;
                    this.$set(this.tabs,this.tabs.length,newItem);
                    return;
                }
            }
        },
        submitTabs:function(){

            this.allTabs = [];

            for(var i = 0; i < this.tabs.length; i++){

                var tabNumber=i+1;
                if(this.tabs[i].title == '') return alert("Title on tab #"+ tabNumber + ' is empty!');
                if(this.tabs[i].price == '') return alert("Price on tab #"+ tabNumber + ' is empty!');
                if(this.tabs[i].quantity == '') return alert("Quantity on tab #"+ tabNumber + ' is empty!');
                if(this.tabs[i].service == '') return alert("Service on tab #"+ tabNumber + ' is empty!');

                this.tabResult = {
                    id: this.tabs[i].id,
                    title: this.tabs[i].title,
                    price: this.tabs[i].price,
                    quantity: this.tabs[i].quantity,
                    service: this.tabs[i].service,
                    totalCost: this.totalCost(this.tabs[i].quantity,this.tabs[i].price)
                };
                this.$set(this.allTabs,i,this.tabResult);    
            }
            this.jsonTabs= JSON.stringify(this.allTabs);
            alert(this.jsonTabs);
        }
    },
    created: function()
    {
        var newItem = Object.assign({}, this.base);
        newItem.id = 0;
        this.$set(this.tabs,0,newItem)
    },
}
</script>
<style>
.widget {
    width: fit-content;
    margin-left: auto;
    margin-right: auto;
}
.forms{
    border-bottom:1px solid;
    border-left: 1px solid;
    border-top: 1px solid;
    border-right: 1px solid;
    border-radius: 5px;
}
.textDiv {
    padding-top: 15px;
    padding-bottom: 15px;
    text-align: left;

}
.buttons{
    padding-bottom:5px;
}
.text{
    width: fit-content;
    display: grid;
    float: left;
    font-size: 20px;
    padding-right: 5px;
}
.inputField{
    display: grid;
}
.bottomButtons{
    margin-right: 10px;
    margin-left: 10px;
}
.total{
    padding-bottom: 10px;
    font-size: 20px;
}
.submit button{
    margin-top: 10px;
    background-color: blue;
    color: white;
}
</style>
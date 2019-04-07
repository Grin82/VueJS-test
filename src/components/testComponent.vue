<template>
    <div>
        <b-tabs pills card content-class="mt-3" v-for="tab in tabs" :key="tab.id" class="widget">
            <b-tab active :title-link-class="[col.tabColor, col.tabText]">
                <template slot="title">
                    <div>{{ title(tab.title, tab.quantity, tab.price) }}</div>
                </template>            
                <div class="textDiv"><div class=text>Enter title </div><div class="inputField"><input type="text" v-model="tab.title"></div></div>
                <div v-for="service in tab.services" :key="service.name" class="buttons">
                    <b-button size="sm" :variant="service.color" @click="col = service">
                        {{service.name}}
                    </b-button>
                    <!-- <br/> -->
                </div>
                <div class="textDiv"><div class=text>Enter quantity </div><div class="inputField"><input type="text" v-model="tab.quantity"></div></div>
               <div class="textDiv">Enter price: <input type="text" v-model="tab.price"><br/></div>
                <div>Total price: {{ totalCost(tab.quantity,tab.price) }}</div>
            </b-tab>
        </b-tabs>
    </div>
</template>

<script>

export default {
    name: 'testComponent',
    data:function(){
        return {
            col:{tabColor:'bg-primary',tebText:'text-light'}, 
            tabs:[{
                id:1,
                title: "",
                amount: "",
                services: [
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
                    }                  
                ],
                quantity: 0,
                price:0,
                totalCost: 0,   
            }]
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
        }

    },
}
</script>
<style>
.widget {
    width: fit-content;
    margin-left: auto;
    margin-right: auto;
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
</style>
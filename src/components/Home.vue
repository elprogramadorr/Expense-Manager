<template>
    <Layout>
        <template v-slot:header>
            <Header></Header>
        </template>
        <template #resume>
            <Resume 
                label="label"
                :amount="amount"
                :totalamount="totalamount"
            >
                <template v-slot:graphic>
                    <Graphic 
                    :amounts="amounts" 
                    />
                </template>
                <template #action>
                    <Action @agregar="agregar"></Action>
                </template>
            </Resume>
        </template>
        <template #movements>
            <Movements 
                :movementstlist="lista">
                @remove="remove"
            </Movements>
        </template>
    </Layout>
</template>

<script>
import Layout from './Layout.vue';
import Header from './Header.vue';
import Resume from './Resume/content.vue';
import Movements from './Movimientos/Index.vue';
import Action from './Action.vue';
import Graphic from './Resume/Graphic.vue';

export default {
    name: 'Home',
    components: {
        Layout,
        Header,
        Resume,
        Movements,
        Action,
        Graphic
    },
    data(){
        return {
            label: 'Amount?',
            amount: null,
            lista: [{
                id: 0,
                tittle: 'tittle1',
                amount: 100,
                description: 'description1',
                time: new Date("12-28-2022")
                },
                {
                id: 1, 
                tittle: 'tittle2',
                amount: 200,
                description: 'description2',
                time: new Date("12-28-2022")
                },
                {
                id: 2,
                tittle: 'tittle3',
                amount: 200,
                description: 'description3',
                time: new Date("12-28-2022")
                },
                {
                id: 3,
                tittle: 'tittle4',
                amount: -1000,
                description: 'description4',
                time: new Date("12-28-2022")
                },
                {
                id: 4,
                tittle: 'tittle5',
                amount: 0,
                description: 'description5',
                time: new Date("12-28-2022")
                },
                {
                id: 5,
                tittle: 'tittle6',
                amount: 10,
                description: 'description6',
                time: new Date("11-22-2022")
                },
                {
                id: 6,
                tittle: 'tittle7',
                amount: 500,
                description: 'description7',
                time: new Date("12-12-2022")
                }
            ]
        }
    },

    computed: {
        amounts(){
            console.log("no tiene cura");
            const filtrado=this.lista.filter((curr)=>{
                const today=new Date();
                const oldDate=today.setDate(today.getDate()-30); 
                return curr.time>=oldDate;
            });
            const res=filtrado.map((curr)=>{
                return curr.amount;
            });
            console.log("numeros",res);
            return res.map((m,i)=>{
                const anteriores=res.slice(0,i);
                const suma=anteriores.reduce((acc,curr)=>{
                    return acc+curr;
                },0);
                return suma+m;                
            });
        },
        totalamount(){
            const res=this.lista.map((curr)=>{
                return curr.amount;
            });
            const suma=res.reduce((acc,curr)=>{
                return acc+curr;
            },0);
            return suma;
        }
    },
    methods: {
        agregar(movement){
            console.log("agregar",movement);
            this.lista.push(movement);
        },
        remove(id){
            const index=this.lista.findIndex((curr)=>{
                return curr.id===id;
            });
            this.lista.splice(index,1);
        },
        pruebita(){
            console.log("prueba");
        }
    },

}
</script>
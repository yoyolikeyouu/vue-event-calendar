<template>
    <div id="app">
       <!-- <h1>vue-event-calendar</h1>
        <h2 class="t-center">Default template</h2>-->
        <vue-event-calendar
                :events="demoEvents"
                @day-changed="handleDayChanged"
                @month-changed="handleMonthChanged"
        ></vue-event-calendar>
        <h2 class="t-center">Custom template</h2>
        <vue-event-calendar :events="demoEvents"
                            @day-changed="handleDayChanged"
                            @month-changed="handleMonthChanged">
            <template scope="props">
                <div v-for="(event, index) in props.showEvents" class="event-item">
                    {{event}}
                </div>
            </template>
        </vue-event-calendar>
    </div>
</template>

<script>
    let today = new Date();
    export default {
        name: 'app',
        data () {
            return {
                demoEvents: [{
                    date: `2019-04-01`,
                    title: 'Title-1',
                    desc: 'longlonglong description',
                } ,{
                    date:`2019-04-08`,
                    title: 'Title-2',
                },{
                    date:`2019-04-10`,
                    title: 'Title-2',
                } ]
            }
        },
        methods: {
            handleDayChanged (data) {
                console.log(data)
            },
            handleMonthChanged (data) {
                console.log(data)
            }
        }
    }
</script>

<style>
    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        color: #2c3e50;
    }

    h1, h2, h3 {
        font-weight: normal;
        margin: 0;
        padding: 0;
    }

    ul {
        list-style-type: none;
        padding: 0;
    }

    li {
        display: inline-block;
        margin: 0 10px;
    }

    a {
        color: #42b983;
    }

</style>

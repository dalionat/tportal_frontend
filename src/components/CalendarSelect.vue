<template>
    <div class="control">
        <div class="field">
        <span class="select is-fullwidth">
        <select v-model="type">
            <option v-for="option in options" :value="option.value">
                {{ option.text }}
            </option>
        </select>
        </span>
        </div>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    name: 'CalendarSelect',
    data() {
        return {
           options: [],
           type: ''
        }
    },
    methods: {
        getCalendars() {
            axios.get('/api/project_mgmt/base/calendars')
            .then(response => {
                response.data.forEach(item => {
                    this.options.push({
                        value: item.project_calendar_id,
                        text: item.title,
                    })
                });
            })
            .catch(error => {
                console.log(error)
            })

        },
    },
    mounted() {
        this.getCalendars();
    },
    computed: {
        
    }
}
</script>

<style>
     select {
        font-family: 'Vazir';
     }
</style>
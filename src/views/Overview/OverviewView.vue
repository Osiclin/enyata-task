<script>
import CardContainer from '../../components/cards/CardContainer.vue';
import PageTitle from '../../components/PageTitle.vue';
import IconTableSelect from '../../components/icons/IconTableSelect.vue';

import { convertDate } from "../../utils"
import { films } from "../../store/films"
import { starships } from "../../store/starships"
import { people } from "../../store/people"
import { species } from "../../store/species"

export default {
    name: "OverviewView",
    components: {
        CardContainer,
        PageTitle,
        IconTableSelect
    },
    data() {
        return {
            tableHead: ['Film Title', 'Release Date', 'Director', 'Producer', 'Episode ID', 'Character'],
            films
        }
    },
    methods: {
        convertDate
    },
    created() {
        if (!films.data.results) films.getFilms()
        if (!starships.data.results) starships.getStarships()
        if (!people.data.results) people.getPeople()
        if (!species.data.results) species.getSpecies()
    }
}
</script>

<template>
    <div class="container">
        <div>
            <CardContainer/>
            <PageTitle title="Films" />
            <div class="tableContainer">
                <table>
                    <thead>
                        <tr>
                            <th><div><IconTableSelect/></div></th>
                            <th v-for="(item, i) in tableHead" :key="i">{{ item }}</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-if="films.loading"><td colspan="7" style="text-align: center;">Loading...</td></tr>
                        <tr 
                            v-else
                            v-for="(item, i) in films.data.results" 
                            :key="i" 
                            @click="this.$router.push({ name: 'overviewDetails', params: { id: i + 1 }})"
                        >
                            <td><IconTableSelect/></td>
                            <td>{{ item.title }}</td>
                            <td>{{ convertDate(item.created) }}</td>
                            <td>{{ item.director }}</td>
                            <td>{{ item.producer }}</td>
                            <td>{{ item.episode_id }}</td>
                            <td>{{ item.characters[0] }}</td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </div>
</template>

<style scoped>
.container{
    padding: 3rem;
}
@media screen and (max-width: 1000px) {
    .container{
        padding: 3rem 1rem;
    }
}
</style>
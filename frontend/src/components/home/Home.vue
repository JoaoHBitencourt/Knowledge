<template>
    <div class="home">
        <PageTitle icon="fa fa-home" main="Dashboard" sub="Base de Conhecimento" />

        <div class="stats">
            <stat title="Categorias" :value="stat.categories" icon="fa fa-folder" color="#e6ba4d" />
            <stat title="Artigos" :value="stat.articles" icon="fa fa-file" color="#00b359" />
            <stat title="Usuários" :value="stat.users" icon="fa fa-user" color="#3282cd" />
        </div>
    </div>
</template>

<script>
import PageTitle from '../template/PageTitle'
import Stat from './Stat'
import axios from 'axios'
import { baseApiUrl } from '@/global'

export default {
    name: 'Home',
    components: { PageTitle, Stat },
    data: function () {
        return {
            stat: {}
        }
    },
    methods: {
        getStats() {
            axios.get(`${baseApiUrl}/stats`).then(res => this.stat = res.data)
        }
    },
    mounted() {
        this.getStats()
    }
}
</script>

<style>
.stats {
    display: flex;
    justify-content: space-between;
    flex-wrap: wrap;
}
</style>
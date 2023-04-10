<template>
    <div>
        <div v-for="url in link" :key="url.id" class="card" @click="showDetails(url.id)">
            <div class="card-body">
                <div class="title-data">
                    <h5>{{ url.title_link }}</h5>
                    <h4>{{ formatDate(url.created_at) }}</h4>
                </div>
                <div class="url">
                    <h5>{{ url.redirect_url }}</h5>
                    <h4>{{ url.maximum_cliks }}</h4>
                </div>
            </div>
        </div>
        <div v-if="selectedUrl">
            <div class="card listaURL">
                <div class="card-body">
                    <div class="title-date-url">
                        <h3>{{ selectedUrl.title_link }}</h3>
                        <h2>{{ formatdate1(selectedUrl.created_at) }}</h2>
                    </div>
                    <p>{{ selectedUrl.redirect_url }}</p>
                </div>
            </div>
            <ul>
                <li v-for="redirect in redirect" :key="redirect.id">{{ redirect.url }}</li>
            </ul>
        </div>
    </div>
</template>
  
<script>
export default {
    name: "App",
    data() {
        return {
            link: [],
            selectedUrl: null,
            redirects: []
        };
    },
    mounted() {
        fetch("http://localhost:8000/")
            .then(response => response.json())
            .then(res => {
                this.link = res;
                console.log(res);
            });
    },
    computed: {
        formatDate() {
            return datetimeStr => datetimeStr.substr(0, 10);
        },
        formatDate() {
            return datetimeStr => {
                const date = new Date(datetimeStr);
                const year = date.getFullYear();
                const month = (date.getMonth() + 1).toString().padStart(2, '0');
                const day = date.getDate().toString().padStart(2, '0');
                return `${day}/${month}/${year}`;
            };
        },
        formatdate1() {
            return datetimeStr => datetimeStr.substr(0, 20);
        },
        formatdate1() {
            return datetimeStr => {
                const date = new Date(datetimeStr);
                const year = date.getFullYear();
                const month = (date.getMonth() + 1).toString().padStart(2, '0');
                const day = date.getDate().toString().padStart(2, '0');
                const hours = date.getHours().toString().padStart(2, '0');
                const minutes = date.getMinutes().toString().padStart(2, '0');
                const seconds = date.getSeconds().toString().padStart(2, '0');
                return `Criado em ${day}/${month}/${year} às ${hours}:${minutes}:${seconds}`;
            };
        },
    },
    methods: {
        showDetails(id) {
            this.selectedUrl = this.link.find(url => url.id === id);
            fetch(`http://localhost:8000/${id}/redirects`)
                .then(response => response.json())
                .then(res => {
                    this.redirects = res;
                    console.log(res);
                });
        }
    }
};
</script>
  

<style scoped>
.card {
    width: 438px;
    position: relative;
    top: -405px;
}

.title-data {
    display: flex;
    gap: 60px;
}

.title-data h5 {
    color: #333333;
    font-family: 'Montserrat';
    font-style: normal;
    font-weight: 600;
    font-size: 18px;
    line-height: 17px;
}

.title-data h4 {
    font-family: 'Montserrat';
    font-style: normal;
    font-weight: 400;
    font-size: 13px;
    line-height: 15px;
    color: #81858E;
}

.url {
    display: flex;
    gap: 200px;
    width: 200px;
    font-family: 'Montserrat';
    font-style: normal;
    font-weight: 400;
    font-size: 15px;
    line-height: 16px;
    color: #81858E;
}

.url h4 {
    font-family: 'Montserrat';
    font-style: normal;
    font-weight: 400;
    font-size: 16px;
    line-height: 17px;
    text-align: right;
    color: #2133D2;
}

/*lado direito da tela mostrando links aninhado com o link principal */

.listaURL {
    width: 560px;
    position: relative;
    left: 480px;
    top: -740px;
}
.title-date-url {
    position: relative;
    display: flex;
    gap: 150px;
}
.title-date-url h2 {
font-size: 18px;
}
</style>

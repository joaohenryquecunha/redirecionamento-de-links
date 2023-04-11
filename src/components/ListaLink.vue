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
                    <div class="url-btn-edit">
                        <p>{{ selectedUrl.redirect_url }}</p>
                        <button class="btn btn-primary btn-sm btnCopiar"
                            @click="copyUrl(selectedUrl.redirect_url)">Copiar</button>
                        <button class="btn btn-primary btn-sm btnEditar" @click="editLink(selectedUrl)">Editar</button>
                    </div>
                    <div v-if="showEditFields">
                        <div class="form-group">
                            <label for="edit-title">Título:</label>
                            <input type="text" class="form-control" id="edit-title" v-model="editTitle">
                        </div>
                        <div class="form-group">
                            <label for="edit-url">URL:</label>
                            <input type="text" class="form-control" id="edit-url" v-model="editUrl">
                        </div>
                        <div class="form-group">
                            <button class="btn btn-primary btn-sm" @click="saveChanges()">Salvar</button>
                            <button class="btn btn-danger btn-sm" @click="cancelEdit()">Cancelar</button>
                        </div>
                    </div>
                </div>
            </div>

            <div class="Sub-links">
                <div v-for="redirect in redirect.filter(item => item.link_id === selectedUrl.id)" :key="redirect.id">
                    <div class="card card-sub">
                        <div class="card-body card-sub-url">
                            <h4>{{ redirect.id }}</h4>
                            <h5>{{ redirect.url }}</h5>
                            <button class="btn btn-primary btn-sm btn-edit-url" @click="editUrl(redirect)">Editar</button>
                        </div>
                    </div>
                </div>
            </div>
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
            redirect: [],
            showEditFields: false,
            editTitle: "",
            editUrl: ""
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
            this.selectedUrl = this.link.find((url) => url.id === id);
            fetch(`http://localhost:8000/redirect/${id}`)
                .then(response => response.json())
                .then(res => {
                    this.redirect = res.filter((item) => item.link_id === id);
                    console.log(res);
                });
        },
        copyUrl(url) {
            const el = document.createElement('textarea');
            el.value = url;
            document.body.appendChild(el);
            el.select();
            document.execCommand('copy');
            document.body.removeChild(el);
        },
        
        editLink(link) {
            this.editTitle = link.title_link;
            this.editUrl = link.redirect_url;
            this.showEditFields = true;
         },

         saveChanges() {
            this.selectedUrl.title_link = this.editTitle;
            this.selectedUrl.redirect_url = this.editUrl;
            this.showEditFields = false;
         },

         cancelEdit() {
            this.showEditFields = false;
         },
    }

};
</script>


  

<style scoped>
.card {
    width: 438px;
    position: relative;
    top: -405px;
    margin-bottom: 5px;
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
    top: -760px;
}

.title-date-url {
    position: relative;
    display: flex;
    gap: 150px;
}

.title-date-url h2 {
    font-size: 18px;
}

.url-btn-edit {
    position: relative;
    display: flex;
    top: 5px;
    gap: 30px;
    font-size: 22px;
}

.btnCopiar {
    position: relative;
    right: -130px;
    height: 40px;
}

.btnEditar {
    position: relative;
    right: -130px;
    height: 40px;
}

/*sessão de sub links, são os links que aparecem ao clicar no link principal*/
.Sub-links {
    position: relative;
    left: 480px;
    top: -300px;
    list-style: none;
    font-size: 20px;
}

.card-sub {
    width: 54%;
    margin-bottom: 5px;
}

.card-sub-url {
    position: relative;
    display: flex;
    gap: 40px;

}

.btn-edit-url {
    position: relative;
    right: -400px;
}
</style>

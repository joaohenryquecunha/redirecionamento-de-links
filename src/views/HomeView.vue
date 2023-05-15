<template>
  <div>
    <button type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#createLinkModal">
      Criar Link
    </button>

    <div class="modal modal-lg" id="createLinkModal" tabindex="-1" aria-labelledby="createLinkModalLabel"
      aria-hidden="true">
      <div class="modal-dialog ">
        <div class="modal-content ">
          <div class="modal-header">
            <h5 class="modal-title" id="createLinkModalLabel">Criar Link</h5>
            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
          </div>
          <div class="modal-body">
            <button type="button" class="btn btn-primary " data-bs-toggle="offcanvas" data-bs-target="#createLinkSidebar">
              Criar Link
            </button>
            <!-- LISTA LINKS -->
            <div class="row">
              <div class="col-4">
                <h2>Links Principais</h2>
                <ul>
                  <li class="card card-body mb-2" v-for="link in links" :key="link.id" @click="getSublinks(link)"
                    :class="{ active: link.id === selectedLinkId }">
                    <p>{{ link.title_link }} {{ link.redirect_url }}</p>
                    <h5>{{ link.maximum_cliks }}</h5>
                  </li>
                </ul>
              </div>
              <div class="col-8">
                <h2 v-if="selectedLink">{{ selectedLink.title_link }}</h2>
                <p v-if="selectedLink">{{ selectedLink.redirect_url }}</p>
                <button v-if="selectedLink" class="btn btn-sm btn-outline-primary mx-2"
                  @click="editLink(selectedLink)">Editar</button>
                <h2>Sublinks</h2>
                <ul>
                  <li class="card card-body" v-for="sublink in sublinks" :key="sublink.id">
                    <a href="#" @click="getClickCount(sublink.id)">{{ sublink.url }} </a> 
                    
                   <span> {{ sublink.max_click }} {{ sublink.current_click }} </span> 
                    <button class="btn btn-sm btn-outline-primary mx-2" @click="editSublink(sublink)">Editar</button>
                  </li>
                </ul>
              </div>
            </div>
            <div class="offcanvas offcanvas-end custom" tabindex="-1" id="createLinkSidebar"
              aria-labelledby="createLinkSidebarLabel">
              <div class="offcanvas-header ">
                <h5 class="offcanvas-title" id="createLinkSidebarLabel">Crie seu Link</h5>
                <button type="button" class="btn-close text-reset " data-bs-dismiss="offcanvas"
                  aria-label="Fechar"></button>
              </div>
              <div class="offcanvas-body">
                <!-- CRIA LINKS -->
                <div class="container">
                  <h1>Criar Link</h1>
                  <form>
                    <div class="form-group">
                      <label for="title_link">Título do Link</label>
                      <input type="text" class="form-control" id="title_link" v-model="title_link">
                    </div>
                    <div class="form-group">
                      <label for="redirect_url">URL de Redirecionamento</label>
                      <input type="text" class="form-control" id="redirect_url" v-model="redirect_url">
                    </div>
                    <div class="form-group" v-for="(sublink, index) in sublinks" :key="index">
                      <h2>Sublink {{ index + 1 }}</h2>
                      <div class="form-group">
                        <label for="url">URL de Redirecionamento</label>
                        <input type="text" class="form-control" v-model="sublink.url">
                      </div>
                      <div class="form-group">
                        <label for="max_click">Número máximo de cliques</label>
                        <input type="number" class="form-control" v-model="sublink.max_click">
                      </div>
                    </div>
                    <button type="button" class="btn btn-primary" @click="addSublink">Adicionar Sublink</button>
                    <button type="button" class="btn btn-primary" @click="saveLink">Salvar Link</button>

                  </form>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
  
<script >

export default {

  data() {
    return {
      links: [],
      sublinks: [],
      selectedLinkId: null,
      selectedLinkCont: null,
      title_link: "",
      redirect_url: "",
      url: "",
      max_click: "",
      current_click:"",
      sublinks: [],
      
    };
  },
  mounted() {
    this.getLinks();
  },
  computed: {
    selectedLink() {
      return this.links.find(link => link.id === this.selectedLinkId);
    }
  },
  methods: {
    
    getLinks() {
      fetch("http://localhost:8000/api/links/")
        .then(response => response.json())
        .then(data => {
          this.links = data.links;
        });
    }, 
    
    getSublinks(link) {
      this.selectedLinkId = link.id;
      fetch(`http://localhost:8000/api/links/${link.id}/sublinks`)
        .then(response => response.json())
        .then(data => {
          this.sublinks = data.sublinks;
        });
    },
    getClickCount(sublinkId) {  
      const selectedLinkCont = this.links.find(link => link.id === this.selectedLinkId);  
  this.selectedSublink = this.sublinks.find(sublink => sublink.id === sublinkId);
  this.selectedSublink.current_click += 1;
  fetch(`http://localhost:8000/api/links/count/${selectedLinkCont.id}/sublinks/${sublinkId}/`, {
    method: "PUT",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify({
      url: this.selectedSublink.url,
      max_click: this.selectedSublink.max_click,
      current_click: this.selectedSublink.current_click,
    }),
  })
    .then(response => response.json())
    .then(data => {
      this.current_click > this.max_click 
      return this.link_default
      // this.getSublinks(this.selectedLinkId);
      
    });
    
    
},


    
    editSublink(sublink) {
      const editedSublink = {
        id: sublink.id,
        url: sublink.url,
        max_click: sublink.max_click
      };
      const newUrl = prompt("Digite o novo URL para o sublink:", sublink.url);
      if (newUrl !== null) {
        editedSublink.url = newUrl;
        const newClick = prompt("Digite a quantidade de cliques:", sublink.max_click);
        if (newClick !== null) {
          editedSublink.max_click = newClick;
        }
        const selectedLink = this.links.find(link => link.id === this.selectedLinkId);
        fetch(`http://localhost:8000/api/links/${selectedLink.id}/sublinks/${sublink.id}`, {
          method: "PUT",
          headers: {
            "Content-Type": "application/json"
          },
          body: JSON.stringify(editedSublink)
        })
          .then(response => {
            if (response.ok) {
              return response.json();
            }
            else {
              throw new Error("Erro ao atualizar sublink");
            }
          })
          .then(data => {
            this.getSublinks(selectedLink);
            this.sublinks = data.sublinks;
            this.getLinks();
          })
          .catch(error => {
            console.error("Erro ao atualizar sublink:", error);
          });
      }
    },

    editLink(link) {
      const editedLink = {
        id: link.id,
        title_link: link.title_link,
        redirect_url: link.redirect_url
      };
      const newTitle = prompt("Digite o novo título para o link:", link.title_link);
      if (newTitle !== null) {
        editedLink.title_link = newTitle;
        const newUrl = prompt("Digite o novo URL de redirecionamento para o link:", link.redirect_url);
        if (newUrl !== null) {
          editedLink.redirect_url = newUrl;
          fetch(`http://localhost:8000/api/links/${link.id}/`, {
            method: "PUT",
            headers: {
              "Content-Type": "application/json"
            },
            body: JSON.stringify(editedLink)
          })
            .then(response => response.json())
            .then(data => {
              this.getLinks();
            })
            .catch(error => {
              console.error("Erro ao atualizar link:", error);
            });
        }
      }
    },
    

    async createLink() {
      try {
        const response = await fetch("http://localhost:8000/api/links/", {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify({ title_link: this.title_link, redirect_url: this.redirect_url })
        });
        const data = await response.json();
        if (response.ok) {
          alert("Link principal criado com sucesso!");
          this.createSublinks(data.link.id);
        }
        else {
          throw new Error(data.message);
        }
      }
      catch (error) {
        alert(`Erro ao criar link principal: ${error.message}`);
      }
    },

    async createSublinks(linkId) {
      try {
        for (const sublink of this.sublinks) {
          const response = await fetch(`http://localhost:8000/api/links/${linkId}/sublinks`, {
            method: "POST",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify({ url: sublink.url, max_click: sublink.max_click })
          });
          const data = await response.json();
          if (!response.ok) {
            throw new Error(data.message);
          }
        }
        alert("Sublinks criados com sucesso!");
        this.title_link = "";
        this.redirect_url = "";
        this.url = "";
        this.max_click = "";
        this.sublinks = [];
        this.getLinks();
      }
      catch (error) {
        alert(`Erro ao criar sublinks: ${error.message}`);
      }
    },

    addSublink() {
      this.sublinks.push({ url: "", max_click: "" });
    },
    async saveLink() {
      try {
        await this.createLink();
      }
      catch (error) {
        alert(`Erro ao salvar link: ${error.message}`);
      }
    },
  },
}
</script>

<style scoped>
.modal.modal-lg {
  width: 100%;
  max-width: 100%;
}

.modal-content {
  position: relative;
  left: -250px;
  width: 1280px;
  margin: 0 auto;
  height: 100vh;
}

.modal-dialog {
  width: 1280px;
}

.custom {
  width: 600px;
}
</style>
  
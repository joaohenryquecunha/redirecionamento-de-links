<template>
  <div>
    <div class="menuStart conteiner conteiner-fluid d-flex flex-column">
    <a href="" data-bs-toggle="modal" data-bs-target="#createLinkModal"><img src="../assets/svgs/icons.svg" alt="icon" class="icon"></a>
    <a href=""><img src="../assets/svgs/icons2.svg" alt="icon2" class="icon2"></a>
    <a href=""><img src="../assets/svgs/icons3.svg" alt="icon3" class="icon3"></a>
    <a href=""><img src="../assets/svgs/icons4.svg" alt="icon4" class="icon4"></a>
    <a href=""><img src="../assets/svgs/icons5.svg" alt="icon5" class="icon5"></a>
    <a href=""><img src="../assets/svgs/icons6.svg" alt="icon6" class="icon6"></a>
  </div>

    <div class="modal modal-lg" id="createLinkModal" tabindex="-1" aria-labelledby="createLinkModalLabel"
      aria-hidden="true">
      <div class="modal-dialog ">
        <div class="modal-content ">
          <div class="modal-header">
            <h5 class="modal-title" id="createLinkModalLabel"></h5>
            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
          </div>
          <div class="modal-body">
            <button type="button" class="btn btn-primary" data-bs-toggle="offcanvas" data-bs-target="#createLinkSidebar" style="position: relative; left: 1100px;">
              Criar Link
            </button>
            <!-- LISTA LINKS -->
            <div class="row">
              <div class="col-4">
                <h4 class="t-principal">Links de Redirecionamento 🌐 </h4>
                <p class="t-frase">Crie seus links de redirect em poucos passos</p>
                <span class="t-click-real">Clique em tempo real</span>
                
                <ul>
                  <li class="card card-body mb-2 links" v-for="link in links"  :key="link.id" @click="getSublinks(link)"
                    :class="{ active: link.id === selectedLinkId }">
                    <span>{{ link.title_link }} {{ formatarData(link.created_at) }}</span>
                    <a href="#" style="text-decoration: none;">{{ link.redirect_url }}</a>
                    <h5 class="max-cliques">{{ link.maximum_cliks }} / {{ getTotalClicks(link.id) }}</h5>
                  </li>
                </ul>
              </div>
              <div class="col-8 link-selecionado">
                <h2 v-if="selectedLink">{{ selectedLink.title_link }}</h2>
                <p v-if="selectedLink">{{ selectedLink.redirect_url }}</p>
                <button v-if="selectedLink" class="btn btn-sm btn-outline-primary mx-2"
                  @click="editLink(selectedLink)">Editar</button>
                <hr style="width: 630px;">
                <ul>
                  <li class="card card-body sublinks" v-for="sublink in sublinks" :key="sublink.id">
                    <a href="#" style="text-decoration: none;" @click="getClickCount(sublink.id)">{{ sublink.url }} </a>

                    <span> {{ sublink.max_click }} / {{ sublink.current_click }} </span>
                    
                    <button class="btn btn-sm btn-outline-primary mx-2" @click="editSublink(sublink)">Editar</button>
                  </li>
                </ul>
              </div>
            </div>
            <div class="offcanvas offcanvas-end custom" tabindex="-1" id="createLinkSidebar"
              aria-labelledby="createLinkSidebarLabel">
              <div class="offcanvas-header " style="background-color: #000; color: #fff;">
                <h5 class="offcanvas-title"  id="createLinkSidebarLabel">Crie seu Link</h5>
                <button type="button" class="btn-close text-reset " data-bs-dismiss="offcanvas"
                  aria-label="Fechar"></button>
              </div>
              <div class="offcanvas-body">
                <!-- CRIA LINKS -->
                <div class="container">
                  <h1>Link Principal</h1>
                  <form>
                    <div class="form-group">
                      <label for="title_link">Título do Link</label>
                      <input type="text" class="form-control" id="title_link" v-model="title_link" required>
                    </div>
                    <div class="form-group">
                      <label for="redirect_url">URL de Redirecionamento</label>
                      <input type="text" class="form-control" id="redirect_url" v-model="redirect_url" required>
                    </div>
                    <div class="form-group" v-for="(sublink, index) in sublinks" :key="index">
                      <h2>SubLink {{ index + 1 }}</h2>
                      <h5>Você poderá inserir uma ou várias URL’s, faça como desejar. </h5>
                      <div class="form-group">
                        <label for="url">URL de Redirecionamento</label>
                        <input type="text" class="form-control" v-model="sublink.url" required>
                      </div>
                      <div class="form-group">
                        <label for="max_click">Número máximo de cliques</label>
                        <input type="number" class="form-control" v-model="sublink.max_click" required>
                      </div>
                    </div>
                    <button type="button" class="btn btn-primary adiciona-sub" @click="addSublink">Adicionar Sublink</button>
                    <button type="button" class="btn btn-primary salva-link" @click="saveLink">Salvar Link</button>

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
      selectedLinkCount: null,
      title_link: "",
      redirect_url: "",
      url: "",
      max_click: "",
      current_click: "",
      totalClick: null,
      crated_at:"",
      quantidadeLinks:0,
      
    };
  },
  mounted() {
    this.getLinks();
    
  },
  computed: {
    selectedLink() {
      return this.links.find(link => link.id === this.selectedLinkId);
    },
    
  },
  
  methods: {

    getLinks() {
      fetch("http://localhost:8000/api/links/")
        .then(response => response.json())
        .then(data => {
          this.links = data.links.reverse();
          this.quantidadeLinks = responseData.links.length;
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
    getTotalClicks(linkId) {
      const sublinksForLink = this.sublinks.filter(sublink => sublink.link_id === linkId);
      const totalClicks = sublinksForLink.reduce((total, sublink) => total + sublink.current_click, 0);
      return totalClicks;
    },
    async getClickCount(sublinkId) {
      const selectedLinkCount = this.links.find(link => link.id === this.selectedLinkId);
      this.selectedSublink = this.sublinks.find(sublink => sublink.id === sublinkId);
      this.selectedSublink.current_click += 1;
      await fetch(`http://localhost:8000/api/links/count/${selectedLinkCount.id}/sublinks/${sublinkId}/`, {
        method: "PUT",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({
          current_click: this.selectedSublink.current_click,
        }),
      });
    },
    calculateTotalClicksForLinks() {
      this.links.forEach(link => {
        link.totalClicks = this.getTotalClicks(link.id);
      });
    },
    formatarData(data) {
      const dataObj = new Date(data);
      const dia = dataObj.getDate().toString().padStart(2, '0');
      const mes = (dataObj.getMonth() + 1).toString().padStart(2, '0');
      const ano = dataObj.getFullYear();
      return `${dia}/${mes}/${ano}`;
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
    //  AQUI ESTA DECLARADO AS FUNÇÕES QUE CRIA LINK E SUBLINKS
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

/* ESTILOS DOS ICONES DA TELA INICIAL */
.menuStart {
  width: 150px;
  height: 700px;  
}

.icon {
  position: absolute;
  width: 35px;
  height: 35px;
  left: 34.48px;
  top: 57.96px;
  border: 2px solid #2133D2;
}
.icon2 {
  position: absolute;
  width: 28px;
  height: 28px;
  left: 37.29px;
  top: 174.67px;
  border: 1.8px solid #81858E;
}
.icon3 {
  position: absolute;
  width: 27px;
  height: 27px;
  left: 38.11px;
  top: 254.3px;
  border: 1.8px solid #81858E;
}
.icon4 {
  position: absolute;
  width: 27px;
  height: 27px;
  left: 38.92px;
  top: 335.56px;
  border: 1.8px solid #81858E;
}
.icon5 {
  position: absolute;
  width: 26px;
  height: 26px;
  left: 38.52px;
  top: 417.63px;
  border: 1.8px solid #81858E;
}
.icon6 {
  position: absolute;
  width: 35px;
  height: 35px;
  left: 31.24px;
  top: 617.03px;
  box-sizing: border-box;
  background: rgba(129, 133, 142, 0.2);
  border: 1px solid #81858E;
}
.icon:hover , .icon2:hover, .icon3:hover, .icon4:hover, .icon5:hover, .icon6:hover{
  border: 2.5px solid #218bd2;
}
/* ESTILOS DOS TÍTULOS DA LISTAGEM DOS LINKS*/
.max-cliques {
  position: relative;
  right: -400px;
  top: -30px;
}
.links {
  top: 50px;
  height: 80px;
  width: 350px;
  padding: 10px;
}
.sublinks {
  position: relative;
  width: 630px;
  right: 30px;
}
.link-selecionado {
  position: relative;
  right: -200px;
}
.t-click-real{
  position: relative;
  top: 40px;
  left: 420px;
  width: 135px;
  height: 15px;
  font-family: 'Montserrat';
  font-style: normal;
  font-weight: 400;
  font-size: 14px;
  line-height: 15px;
  display: flex;
  align-items: center;
  letter-spacing: 0.2px;
  color: #81858E;
}
/*BOTÃO SALVAR LINK*/
.salva-link {
  position: relative;
  left: -150px;
  top: 70px;
}
.adiciona-sub {
  position: relative;
  top: 10px;
}

</style>
  
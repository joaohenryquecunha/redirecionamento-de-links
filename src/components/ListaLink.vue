<template>
  <div class="row">
    <div class="col-4">
      <h2>Links Principais</h2>
      <ul>
        <li class="card card-body mb-2" v-for="link in links" :key="link.id" @click="getSublinks(link)" :class="{ active: link.id === selectedLinkId }">
         <p>{{ link.title_link }}  {{ link.redirect_url }}</p> 
        </li>
      </ul>
    </div>
    <div class="col-8">
      <h2 v-if="selectedLink">{{ selectedLink.title_link }}</h2>
      <p v-if="selectedLink">{{ selectedLink.redirect_url }}</p>
      <button v-if="selectedLink" class="btn btn-sm btn-outline-primary mx-2" @click="editLink(selectedLink)">Editar</button>
      <h2>Sublinks</h2>
      <ul>
        <li class="card card-body" v-for="sublink in sublinks" :key="sublink.id">
          {{ sublink.url }} {{ sublink.max_click }}
          <button class="btn btn-sm btn-outline-primary mx-2" @click="editSublink(sublink)">Editar</button>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>


export default {
  data() {
    return {
      links: [],
      sublinks: [],
      selectedLinkId: null
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
      fetch('http://localhost:8000/api/links/')
        .then(response => response.json())
        .then(data => {
          this.links = data.links;
        });
    },
    getLinksSub() {
      fetch('http://localhost:8000/api/links/{id}/sublinks')
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
    editSublink(sublink) {
  const editedSublink = {
    id: sublink.id,
    url: sublink.url,
    max_click: sublink.max_click
  };
  const newUrl = prompt('Digite o novo URL para o sublink:', sublink.url);
  if (newUrl !== null) {
    editedSublink.url = newUrl;
    const newClick = prompt('Digite a quantidade de cliques:', sublink.max_click);
    if (newClick !== null) {
      editedSublink.max_click = newClick;
    }
    const selectedLink = this.links.find(link => link.id === this.selectedLinkId); 
    fetch(`http://localhost:8000/api/links/${selectedLink.id}/sublinks/${sublink.id}`, {
  method: 'PUT',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify(editedSublink)
})
  .then(response => {
    if (response.ok) {
      return response.json();
    } else {
      throw new Error('Erro ao atualizar sublink');
    }
  })
  .then(data => {
    getLinksSub()
    
    })
  .catch(error => {
    console.error('Erro ao atualizar sublink:', error);
  });

  }
},
    editLink(link) {
      const editedLink = {
        id: link.id,
        title_link: link.title_link,
        redirect_url: link.redirect_url
      };
      const newTitle = prompt('Digite o novo título para o link:', link.title_link);
      if (newTitle !== null) {
        editedLink.title_link = newTitle;
        const newUrl = prompt('Digite o novo URL de redirecionamento para o link:', link.redirect_url);
        if (newUrl !== null) {
          editedLink.redirect_url = newUrl;
          fetch(`http://localhost:8000/api/links/${link.id}/`, {
            method: 'PUT',
            headers: {
              'Content-Type': 'application/json'
            },
            body: JSON.stringify(editedLink)
          })
            .then(response => response.json())
            .then(data => {
              this.getLinks();
            })
            .catch(error => {
              console.error('Erro ao atualizar link:', error);
            });
        }
      }
    }
  }
};
</script>


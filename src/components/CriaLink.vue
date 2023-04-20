<template>
  <div>
    <h2>Salvar link</h2>
    <form @submit.prevent="CreateLink">
      <label for="titulo-link">Título do link:</label>
      <input type="text" id="titulo-link" v-model="tituloLink"><br><br>
      <label for="url-original">URL original:</label>
      <input type="text" id="url-original" v-model="urlOriginal"><br><br>
      <button type="submit">Salvar</button>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tituloLink: '',
      urlOriginal: ''
    }
  },
  methods: {
    CreateLink() {
  fetch('http://localhost:8000/', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({ title_link: this.tituloLink, redirect_url: this.urlOriginal })
  })
    .then(response => {
      if (!response.ok) {
        throw new Error('Erro ao salvar os dados');
      }
      return response.json();
    })
    .then(data => {
      console.log('Dados salvos com sucesso:', data);
    })
    .catch(error => {
      console.error(error);
    });
}

  }
}
</script>

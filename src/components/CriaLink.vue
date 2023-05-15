<template>
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
</template>


<script setup>
    import { ref } from 'vue'

    const title_link = ref('')
    const redirect_url = ref('')
    const url = ref('')
    const max_click = ref('')
    const sublinks = ref([])

    const createLink = async () => {
        try {
            const response = await fetch("http://localhost:8000/api/links/", {
                method: "POST",
                headers: {"Content-Type": "application/json"},
                body: JSON.stringify({title_link: title_link.value, redirect_url: redirect_url.value})
            })
            const data = await response.json()
            if (response.ok) {
                alert("Link principal criado com sucesso!")
                await createSublinks(data.link.id)
            } else {
                throw new Error(data.message)
            }
        } catch (error) {
            alert(`Erro ao criar link principal: ${error.message}`)
        }
    }

    const createSublinks = async (linkId) => {
        try {
            for (const sublink of sublinks.value) {
                const response = await fetch(`http://localhost:8000/api/links/${linkId}/sublinks`, {
                    method: "POST",
                    headers: {"Content-Type": "application/json"},
                    body: JSON.stringify({url: sublink.url, max_click: sublink.max_click})
                })
                const data = await response.json()
                if (!response.ok) {
                    throw new Error(data.message)
                }
            }
            alert("Sublinks criados com sucesso!")
            $emit('atualizar')
            clearForm()
        } catch (error) {
            alert(`Erro ao criar sublinks: ${error.message}`)
        }
    }

    const addSublink = () => {
        sublinks.value.push({url: "", max_click: ""})
    }

    const clearForm = () => {
        title_link.value = ""
        redirect_url.value = ""
        url.value = ""
        max_click.value = ""
        sublinks.value = []
    }
    
    const saveLink = async () => {
        try {
            await createLink()
            
        } catch (error) {
            alert(`Erro ao salvar link: ${error.message}`)
        }
    }  
    
    
            
        
    
</script>




<style>
    .container {
        margin-top: 50px;
    }
</style>
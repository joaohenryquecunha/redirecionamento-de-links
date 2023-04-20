
<template>
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
                    <!-- botão para abrir janela lateral -->
                    <button type="button" class="btn btn-primary " data-bs-toggle="offcanvas"
                        data-bs-target="#createLinkSidebar">
                        Criar Sublink
                    </button>
                    <ListaLink />
                    <div class="offcanvas offcanvas-end" tabindex="-1" id="createLinkSidebar"
                        aria-labelledby="createLinkSidebarLabel">
                        <div class="offcanvas-header ">
                            <h5 class="offcanvas-title" id="createLinkSidebarLabel">Criar Sublink</h5>
                            <button type="button" class="btn-close text-reset " data-bs-dismiss="offcanvas"
                                aria-label="Fechar"></button>
                        </div>
                        <div class="offcanvas-body">
                            <!-- conteúdo da sidebar aqui -->
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>


<script setup>
import ListaLink from '../components/ListaLink.vue';


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
</style>
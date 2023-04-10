<template>
    <el-button class="icon" @click="visible = true">
        <img src="../assets/svgs/icons.svg" alt="">
    </el-button>
    <ElButton class="icon2"><img src="../assets/svgs/icons2.svg" alt=""></ElButton>
    <ElButton class="icon3"><img src="../assets/svgs/icons3.svg" alt=""></ElButton>
    <ElButton class="icon4"><img src="../assets/svgs/icons4.svg" alt=""></ElButton>
    <ElButton class="icon5"><img src="../assets/svgs/icons5.svg" alt=""></ElButton>
    <ElButton class="icon6"><img src="../assets/svgs/icons6.svg" alt=""></ElButton>
    <el-dialog width="90%" height="100%" v-model="visible" :show-close="false">
        
        <template #header="{ titleId, titleClass }">
            <div class="my-header">
                <h2 :id="titleId" :class="titleClass" style="margin-top: 10px;">Links de Redirecionamento 🌐 </h2>
                <h2 class="txtlink">Crie seus links de redirect em poucos passos</h2>

            </div>
            <hr class="divisaoH1">
            <hr class="divisaoH4">
            <hr class="divisaoV">

            <el-button class="crialink" style="margin-left: 16px" @click="drawer = true">
                Criar um Link
            </el-button>

            <el-drawer v-model="drawer" width="100%" title="I am the title" :with-header="false">
                <h4 class="txtCriacaoLink">Criação de Link</h4>
                <div class="CrieSuaUrl">
                    <label class="titulolink" for="">Titulo do link</label>
                    <input class="DadosInput" type="text" placeholder="insira um título">
                </div>
                <h2 class="urlOriginal">URL ORIGINAL</h2>
                <P class="paragrafo1">Você poderá inserir uma ou várias URL’s, faça como desejar. Lembre-se de inserir a quantidade de cliques junto à URL.</P>
                <div class="subLink1">
            <h5 class="urlid1">01</h5>
            <h5 class="enderecoURL1">https://www.redirec/g-digital-id=1-0105&t=bc19gdig580142</h5>
            <div class="contadorURL1">
                <p>05/500</p>
            </div>
                 </div>
                 <div>
                     <input class="addlink" type="text" placeholder="Insira a URL original">
                     <input class="addclick" type="number" placeholder="qtd cliques">
                     <ElButton class="btnadd">+ Adicionar mais URL</ElButton>
                 </div>
                 <h2 class="urlDefault">URL Default</h2>
                 <p class="textoDefault">Essa URL será associada ao redirecionamento apenas quando todas as outras chegarem ao limite de cliques. Ela será a uma url fixa sem limitação.</p>
                 <div><input class="inputDefault" type="text" placeholder="Insira a URL Default"></div>
                 <ElButton class="btnSalvar">Salvar Link</ElButton>
            </el-drawer>
        </template>
        <ListaLink/>
        <div class="listaurl">
            <h5 class="link">LINK 01 </h5>
            <h5 class="data">Criado em: 01/04/2023 às 10:36</h5>
            <h4 class="url"></h4>
            <ElButton class="btnurlC">Copiar</ElButton>
            <ElButton class="btnurlE">Editar</ElButton>

        </div>

        <div class="subLinks">
            <h5 class="urlid">01</h5>
            <h5 class="enderecoURL">https://www.redirec/g-digital-id=1-0105&t=bc19gdig580142</h5>
            <ElButton class="btnediturl">Editar</ElButton>
            <div class="contadorURL">
                <p>05/500</p>
            </div>
        </div>

    </el-dialog>
</template>
  
<script setup>
import { ref } from 'vue'
import { ElButton, ElDialog } from 'element-plus'
import ListaLink from '../components/ListaLink.vue';
const drawer = ref(false)
const visible = ref(false)
</script>
<script>
export default {
    name: "App",
    data() {
        return {
            link: [],
        };
    },
    mounted() {
        fetch("http://localhost:8000/")
            .then(response => response.json())
            .then((res) => {
            this.link = res;
            return console.log(res);
        });
    },
    components: { ListaLink }
}
</script>
 
<style scoped>
.my-header {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    height: 520px;
    width: 1185px;

}

.el-dialog__wrapper {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
    width: 100%;
    height: 100%;
}

.el-dialog {
    margin: 0;
    width: 100%;
    height: 100%;
    max-height: 100%;
}

.el-dialog__body {
    margin: 0;
    padding: 0;
    height: 100%;
    max-height: 100%;
    overflow-y: auto;
}


/*estilos e posicionamento dos icones*/
.icon {
    position: absolute;
    width: 25px;
    height: 25px;
    left: 34.48px;
    top: 57.96px;
    color: #2133D2;

}

.icon2 {
    position: absolute;
    width: 22.9px;
    height: 19.62px;
    left: 26.29px;
    top: 174.67px;

}

.icon3 {
    position: absolute;
    width: 21.26px;
    height: 21.26px;
    left: 26.11px;
    top: 254.3px;

}

.icon4 {
    position: absolute;
    width: 19.63px;
    height: 21.26px;
    left: 26.92px;
    top: 335.56px;

}

.icon5 {
    position: absolute;
    width: 20.44px;
    height: 20.44px;
    left: 26.52px;
    top: 417.63px;

}

.icon6 {
    position: absolute;
    width: 35px;
    height: 35px;
    left: 29.24px;
    top: 550.03px;
    box-sizing: border-box;
    background: rgba(129, 133, 142, 0, 2);
}

.icon:hover,
.icon2:hover,
.icon3:hover,
.icon4:hover,
.icon5:hover,
.icon6:hover {
    border: solid 2px #8d8de8
}

/* personalização do subtitulo de links*/
.txtlink {
    position: absolute;
    display: block;
    top: 60px;
    font-size: 15px;
    color: #81858E;
    font-family: 'Montserrat';

}

/*botão Criar Link posicionamento*/
.crialink {
    position: absolute;
    top: 30px;
    right: 100px;
    width: 202.58px;
    height: 45px;
    background: rgba(33, 51, 210, 0.1);
    border-radius: 5px;
    color: #2133D2;
    font-family: Montserrat;
    font-style: normal;
    font-weight: 600;
    font-size: 18px;
    line-height: 17px;
}

/*estilos do texto Clique em tempo Real*/
.txtCliqueReal {
    width: 135px;
    height: 15px;
    margin-top: 10px;
    margin-left: 380px;
    color: #81858E;
    font-size: 14px;
    font-family: 'Montserrat';
    font-style: normal;
    font-weight: 400;
    font-size: 14px;
    line-height: 15px;
}

/*estilos do texto referente a quantidade de links*/
.qtdLink {
    position: absolute;
    left: 10px;
    top: 10px;
    color: #2133D2;
    font-family: 'Montserrat';
    font-style: normal;
    font-weight: 600;
    font-size: 25px;
    line-height: 17px;
}

.conteinerLinks {
    display: flex;
    position: absolute;
    top: 140px;
}

/* personalização conteiner com os links principais*/


.link {
    font-family: 'Montserrat';
    font-style: normal;
    font-weight: 600;
    font-size: 15px;
    line-height: 17px;
    color: #191B28;
    cursor: pointer;
}

.link:hover {
    color: #2133D2;
}

.data {
    font-family: 'Montserrat';
    font-style: normal;
    font-weight: 400;
    font-size: 15px;
    line-height: 15px;
}

.url {

    position: absolute;
    top: 30px;
    font-family: 'Montserrat';
    font-style: normal;
    font-weight: 400;
    font-size: 18px;
    line-height: 16px;

}

/*linhas de separação dos conteiners*/
.divisaoH1 {
    position: relative;
    top: -440px;
    background: #E5E5E5;

}

.divisaoV {
    position: absolute;
    width: 518.66px;
    height: 0.5px;
    left: 199px;
    top: 360px;
    background: #E5E5E5;
    transform: matrix(0, -1, -1, 0, 0, 0);
}

.divisaoH2 {
    position: absolute;
    width: 540px;
    height: 0.5px;
    left: 20px;
    top: 190px;
    background: #EDEDF0;
}

.divisaoH3 {
    position: absolute;
    width: 540px;
    height: 0.5px;
    left: 20px;
    top: 270px;
    background: #EDEDF0;
}

.divisaoH4 {
    position: absolute;
    width: 880px;
    height: 0.5px;
    left: 500px;
    top: 270px;
    background: #EDEDF0;
}

/*contador de cliques*/
.contador {
    position: absolute;
    left: 430px;
    top: 240px;
    color: #2133D2;
    font-size: 20px;
    font: 100;

}

/* configurações da lista de URL separada pelo Link principal*/
.listaurl {
    position: absolute;
    display: flex;
    top: 220px;
    left: 600px;
    bottom: 0px;
    gap: 50px;
    color: #2133D2;
}

.btnurlC {
    position: absolute;
    left: 230px;
    top: 20px;
    background: rgba(33, 51, 210, 0.1);
    border-radius: 3px;
}

.btnurlE {
    position: absolute;
    left: 330px;
    top: 20px;
}

.subLinks {
    display: flex;
    gap: 40px;
    position: absolute;
    top: 330px;
    right: 260px;
}

.btnediturl {
    position: absolute;
    right: -150px;
    top: -2px;
}

.contadorURL {
    position: absolute;
    top: 40px;
    left: 60px;
}

/*configurações da ultima página do modal onde Cria as url referente aos links*/
.txtCriacaoLink {

    margin-top: -20px;
    width: 470px;
    background-color: #191B28;
    color: #fff;
    padding: 20px;
    font-family: Montserrat;
    font-style: normal;
    font-weight: 600;
    font-size: 16px;
    line-height: 20px;
}
.urlid1 {
    position: absolute;
    width: 14px;
    height: 16px;
    left: 34px;
    top: 320px;

    font-family: 'Montserrat';
    font-style: normal;
    font-weight: 600;
    font-size: 13px;
    line-height: 16px;
    display: flex;
    align-items: center;
    letter-spacing: 0.2px;
    color: #000000;

}
.enderecoURL1 {
    position: absolute;
    width: 356px;
    height: 32px;
    left: 68px;
    top: 312px;

    font-family: 'Montserrat';
    font-style: normal;
    font-weight: 400;
    font-size: 13px;
    line-height: 16px;
    display: flex;
    align-items: center;
    letter-spacing: 0.2px;
    color: #81858E;
}
.contadorURL1{
    position: absolute;
    width: 31.72px;
    height: 16px;
    left: 420px;
    top: 328px;

    font-family: 'Montserrat';
    font-style: normal;
    font-weight: 400;
    font-size: 13px;
    line-height: 16px;
    display: flex;
    align-items: center;
    letter-spacing: 0.2px;
    color: #81858E;
}
.CrieSuaUrl {
    position: absolute;
    top: 100px;
}
.DadosInput {
    width: 310px;
    position: absolute;
    top: 40px;
    left: 10px;
    
}
.urlOriginal {
    position: absolute;
    top: 200px;
    font-size: 15px;
    color: #2133D2;
}
.paragrafo1 {
    position: absolute;
    top: 230px;
    font-size: 14px;
}
.addlink {
    width: 310px;
    position: absolute;
    top: 380px;
}
.addclick {
    width: 100px;
    position: absolute;
    top: 380px;
    left: 350px;
}
.btnadd {
    position: absolute;
    top: 420px;
    color: #2133D2;
}
.urlDefault {
    position: absolute;
    top: 490px;
    font-size: 18px;
    color: #2133D2;
}
.textoDefault {
    position: absolute;
    top: 520px;
    font-size: 14px;
}
.inputDefault {
    width: 310px;
    position: absolute;
    top: 580px;
}
.titulolink {
    width: 120px;
    position: absolute;
    left: 10px;
    top: 10px;
}
.btnSalvar {
    width: 200px;
    position: absolute;
    top: 700px;
    right: 25px;
    font-size: 20px;
    background: #2133D2;
    color: #fff;
}
</style>
   
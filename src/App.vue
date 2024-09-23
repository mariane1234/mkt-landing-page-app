<script setup>

import { ref } from 'vue'

const formData = ref({
  "name":"",
  "email":""
})

const step = ref(1)

const profilingData = ref({
  "leadId":null,
  "source":"",
  "target":"",
  "business":"",
  "businessDescription":"",
})

const fillData = (e) => {
  let formValue = e.target.value
  let formRef = e.target.name

  formData.value[formRef] = formValue
}

const fillProfilingData = (e) => {
  let formValue = e.target.value
  let formRef = e.target.name

  profilingData.value[formRef] = formValue
}

const action = async () => {
  if(step.value == 1){
    try {
      const response = await fetch('http://localhost:8000/leads', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(formData.value)
      });

      if (!response.ok) {
        throw new Error('Erro na requisição');
      }

      const data = await response.json();
      profilingData.value.leadId = data.id
      console.log(profilingData.value)
    } catch (error) {
      console.error('Erro:', error);
    }
  }else if(step.value == 2){
    try {
      const response = await fetch('http://localhost:8000/leads/profiling', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(profilingData.value)
      });

      if (!response.ok) {
        throw new Error('Erro na requisição');
      }

      const data = await response.json();
      console.log(data)
    } catch (error) {
      console.error('Erro:', error);
    }
  }

  step.value++
}

</script>

<template>
  <div class="grid grid-cols-[1fr_2fr]">
    <div class="w-full h-screen bg-blue-100 p-10">
        <div class="flex flex-col gap-4 justify-center w-full h-full" v-if="step == 1">
          <div class="flex justify-center">
            <img class="ls-img" src="/public/assets/arrow.png" alt=""/>
          </div>
          <p class="font-sans text-md font-semibold text-indigo-900 mb-5">Transforme seu negócio hoje mesmo com estratégias de marketing que levam sua marca ao próximo nível!.</p>
          <input autocomplete="off" class="w-full h-12 rounded-md placeholder:px-2" type="text" name="name" id="input-name" placeholder="Nos informe seu nome" :value="formData.name" @input="event => fillData(event)">
          <input autocomplete="off" class="w-full h-12 rounded-md placeholder:px-2" type="email" name="email" id="input-email" placeholder="Indique seu melhor e-mail" :value="formData.email" @input="event => fillData(event)">
          <button class="mt-4 rounded-md px-3 py-2 bg-indigo-600 text-white font-semibold" type="button" @click="action">Quero me manter informado!</button>
        </div>
        <div class="flex flex-col gap-3 justify-center w-full h-full" v-if="step == 2">
          <span class="font-sans font-bold text-lg">Que legal que você se interessou, {{ formData.name }}! </span>
          <span class="font-sans text-gray-600 font-semibold text-sm mb-5">Nos conte um pouco mais sobre você para que possamos pensar em uma estratégia juntos.</span>
          <div class="">
            <label for="source-select" class="block mb-2 text-sm font-medium text-gray-900">Onde nos encontrou?</label>
            <select id="source-select" name="source" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5"
                    @change="event => fillProfilingData(event)"
            >
              <option selected disabled>-- Selecione uma opção --</option>
              <option value="linkedin">Pelo LinkedIn</option>
              <option value="instagram">Pelo Instagram</option>
              <option value="google">Pelo Google</option>
              <option value="outro">De outra forma</option>
            </select>
          </div>
          <div class="">
            <label for="business-select" class="block mb-2 text-sm font-medium text-gray-900">Qual seu setor de atuação?</label>
            <select id="business-select" name="business" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5"
                    @chage="event => fillProfilingData(event)"
            >
              <option selected disabled>-- Selecione uma opção --</option>
              <option value="imobiliario">Imobiliário</option>
              <option value="saude">Saúde e Bem-estar</option>
              <option value="ecommerce">E-commerce</option>
              <option value="outro">Outro</option>
            </select>
          </div>
          <div class="">
            <label for="target-select" class="block mb-2 text-sm font-medium text-gray-900">Qual o seu público-alvo?</label>
            <select id="target-select" name="target" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5"
                  @chage="event => fillProfilingData(event)"
            >
              <option selected disabled>-- Selecione uma opção --</option>
              <option value="jovens">Jovens</option>
              <option value="adultos">Adultos</option>
              <option value="idosos">Idosos</option>
              <option value="outro">Outro público-alvo</option>
            </select>
          </div>
          <div class="">
            <label for="businessDescription" class="block mb-2 text-sm font-medium text-gray-900">Qual o seu negócio?</label>
            <textarea id="businessDescription" name="businessDescription" rows="3" 
              class="block p-2.5 w-full text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:ring-blue-500 focus:border-blue-500" 
              placeholder="Nos conte um pouco mais sobre seu negócio"
              :value="profilingData.businessDescription" @input="event => fillProfilingData(event)"
              ></textarea>
          </div>
          <button class="mt-4 rounded-md px-3 py-2 bg-indigo-600 text-white font-semibold" type="button" @click="action">Quer saber mais!</button>

        </div>
        <div class="flex flex-col gap-2 w-full h-full justify-center items-center" v-if="step == 3">
          <span class="text-3xl font-semibold text-indigo-800 mb-3">Esteja pronto para o sucesso! 🚀</span>
          <p class="text-gray-700">Tomar a decisão de trabalhar com esta agência de marketing pode ser o grande impulso que seu negócio precisa! Com estratégias criativas e personalizadas, 
            o foco está em gerar resultados reais e elevar sua marca. Conte com um time dedicado a transformar suas metas em conquistas e a fazer seu negócio brilhar.</p>
        </div>
    </div>
    <div class="w-full h-screen bg-white">

      <div class="grid grid-cols-2 gap-4 w-full h-full justify-center items-center">
        <div class="flex flex-col justify-center items-center w-full h-full p-4">
          <span class="font-extrabold	font-sans text-2xl text-indigo-800 mb-2 text-right">
            Divulgue sua marca utilizando as melhores estratégias
          </span>
          <span class="text-right">
            Divulgue sua marca com as melhores estratégias de marketing digital e conquiste seu espaço. Usamos SEO, mídia paga, 
            redes sociais e conteúdo para garantir mais visibilidade e resultados, transformando sua empresa em referência no setor.  
          </span>
        </div>
        <div class="flex justify-center items-center  w-full h-full p-4">
          <img class="lp-mkt-img" src="/public/assets/megaphone.jpg" alt=""/>
        </div>
        <div class="flex justify-center items-center w-full h-full p-4">
          <img class="lp-mkt-img" src="/public/assets/handshake.jpg" alt=""/>
        </div>
        <div class="flex flex-col justify-center items-start w-full h-full p-4">
          <span class="font-extrabold	font-sans text-2xl text-indigo-800 mb-2">
            Encontre oportunidades de alavancar seu negócio
          </span>
          <span>
            Encontre oportunidades de alavancar seu negócio com estratégias personalizadas para expandir sua presença digital e aumentar suas vendas. 
            Aproveite cada chance de crescimento com ações focadas em resultados.
          </span>
        </div>
      </div>        
    </div>
  </div>
</template>

<style scoped>
img.ls-img{
  width: 200px;
  height: fit-content;
}
img.lp-mkt-img{
  width: 30vw;
  height: fit-content;
}
</style>

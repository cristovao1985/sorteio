<template>
  <q-page class="text-center">
    <q-card class="my-card q-ma-xl q-pa-xl">
      <div v-if="mostrarApresentacao">
        <h3>Vamos iniciar um novo sorteio?</h3>
        <h6>Informe qual o número máximo para sorteio</h6>
        <div class="row justify-center">
          <div class="col-2">
            <q-input
              class="q-ma-xl"
              outlined
              label="Número máximo"
              placeholder="Qual o número máximo para ser sorteado?"
              min="5"
              autofocus
              v-model="numeroMaximo"
              mask="#####"
              :input-style="{ fontSize: '25px' }"
            />
          </div>
        </div>
      </div>
      <div class="text-center" v-if="!mostrarApresentacao">
        <q-banner class="bg-grey-3 q-mb-md">
          <template v-slot:avatar>
            <q-icon name="celebration" color="primary" />
          </template>
          <v-container fluid>
            <h3 class="text-primary">
              Números sorteados de 1 à {{ numeroMaximo }}
            </h3>
            <h4 class="text-bold">{{ numerosSorteados }}</h4>
          </v-container>
          <template v-slot:action>
            <q-btn
              flat
              color="positive"
              label="Novo sorteio"
              @click="resetarSorteio"
              icon="refresh"
            />
          </template>
        </q-banner>
      </div>
      <q-btn
        unelevated
        rounded
        color="primary"
        label="Sortear número!"
        size="lg"
        icon="card_giftcard"
        @click="sortearNumero"
      />
    </q-card>
    <q-dialog v-model="modal.numero">
      <q-card class="my-card" style="width: 50%">
        <q-card-section class="text-center flex-center">
          <h3>O número sorteado foi</h3>
          <h1 class="text-negative text-bold">
            {{ numeroSorteado }}
          </h1>
        </q-card-section>
        <q-card-actions align="right">
          <q-btn
            label="Ok"
            @click="fecharModal('numero')"
            flat
            color="secondary"
          />
        </q-card-actions>
      </q-card>
    </q-dialog>
  </q-page>
</template>

<script>
import { defineComponent } from "vue";

export default defineComponent({
  name: "IndexPage",
  data() {
    return {
      numeroMaximo: 50,
      numerosSorteados: [],
      numeroSorteado: null,
      modal: {
        numero: false,
      },
    };
  },
  computed: {
    mostrarApresentacao() {
      return !this.numerosSorteados.length;
    },
  },
  methods: {
    abrirModal(modal) {
      this.modal[modal] = true;
    },
    fecharModal(modal) {
      this.modal[modal] = false;
    },
    sortearNumero() {
      let novoNumero = Math.floor(Math.random() * this.numeroMaximo) + 1; // Sorteia um número de 1 a 10

      while (this.numerosSorteados.includes(novoNumero)) {
        novoNumero = Math.floor(Math.random() * this.numeroMaximo) + 1; // Sorteia novo número se já foi sorteado
      }

      this.numeroSorteado = novoNumero;
      this.numerosSorteados.push(novoNumero);

      this.abrirModal("numero");
    },
    resetarSorteio() {
      this.numerosSorteados = [];
      this.numeroSorteado = null;
      this.numeroMaximo = 50;
    },
  },
});
</script>

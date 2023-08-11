<template>
  <div class="container">
    <div v-if="state.total != 0">
      <h2>Lo que terminarias pagando por tu prestamo seria:</h2>
      <h4 class="total">$ {{ state.total }}</h4>
    </div>
    <div class="calculadora">
      <img src="../assets/credito-illus.png" alt="credito illus" />
      <form class="form">
        <label class="titulo_campo" name="Monto"
          >Monto del Prestamo <sup class="sup">*</sup></label
        >
        <input
          ref="monto"
          v-model="state.monto"
          class="campo"
          type="number"
          id="monto"
          name="Monto"
          placeholder="Monto del prestamo"
        />
        <span class="error" v-if="v$.monto.$error"
          >El valor del monto no es valido</span
        >
        <label class="titulo_campo" name="Cuotas"
          >Plazo en meses <sup class="sup">*</sup></label
        >
        <input
          ref="cuotas"
          v-model="state.cuotas"
          class="campo"
          type="number"
          id="cuotas"
          name="Cuotas"
          placeholder="Plazo en meses"
        />
        <span class="error" v-if="v$.cuotas.$error"
          >El plazo en meses no es valido</span
        >
        <label class="titulo_campo" name="Interes"
          >Taza de interes <sup class="sup">*</sup></label
        >
        <input
          ref="interes"
          class="campo"
          type="number"
          v-model="state.interes"
          id="interes"
          name="Interes"
          placeholder="Taza de interes"
        />
        <span class="error" v-if="v$.interes.$error"
          >La taza de interes no es valido</span
        >
        <div class="buttons">
          <button type="button" v-on:click="calcular" class="btn">
            Calcular
          </button>
          <button type="reset" v-on:click="reset" class="btn btn-reset">
            Reiniciar
          </button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import useValidate from "@vuelidate/core";
import { required, between, integer } from "@vuelidate/validators";
import { reactive, computed } from "vue";

const validateMonto = (value) => value > 0;

export default {
  name: "Calculadora Prestamos",
  setup() {
    const state = reactive({
      monto: "Monto del prestamo",
      cuotas: "Plazo de pago",
      interes: "Taza de interes",
      total: 0,
    });

    const rules = computed(() => {
      return {
        monto: { required, validateMonto },
        cuotas: { required, validateMonto, integer },
        interes: { required, between: between(0, 100) },
      };
    });

    const v$ = useValidate(rules, state);

    return {
      state,
      v$,
    };
  },
  methods: {
    calcular() {
      this.v$.$validate();
      if (!this.v$.$error) {
        this.state.total =
          this.state.monto +
          (this.state.monto * this.state.interes * this.state.cuotas) / 100;
      }
    },
    reset() {
      this.state.monto = "";
      this.state.cuotas = "";
      this.state.interes = "";
      this.state.total = "";
    },
  },
};
</script>

<style scoped>
.total {
  font-weight: bold;
  font-size: 4rem;
  color: #4c39ff;
  border: solid 1px #4c39ff;
  width: 50%;
  margin: 2rem auto;
}
.form {
  display: flex;
  flex-direction: column;
  width: 50%;
  margin: auto 1rem;
}
.buttons {
  display: flex;
  justify-content: space-between;
  width: 50%;
  margin: 2rem auto;
}
.campo {
  margin-top: 0.3rem;
  height: 2rem;
  border: solid 1px;
  border-radius: 0.5rem;
  border-color: #4c39ff;
}
.titulo_campo {
  text-align: left;
  font-weight: bold;
  margin-top: 1.5rem;
}
.btn {
  color: #fff;
  background: #4c39ff;
  padding: 1.2rem;
  border-radius: 0.5rem;
  border: none;
  text-align: center;
}
.btn:hover {
  background: #27cf71;
}
.btn-reset {
  background: #ff593e;
}
.btn-reset:hover {
  background: #004051;
}
.sup {
  color: #ff593e;
}
.error {
  color: #ff593e;
  margin-top: 0.5rem;
}
.calculadora_ilustarcion {
  position: absolute;
  z-index: -1;
  right: 38rem;
  top: 15rem;
}
.calculadora {
  display: flex;
  justify-content: space-around;
  margin: 0 3rem;
}
</style>

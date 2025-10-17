<<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <title>Forca em Vue.js (Básico)</title>
    <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; }
        .palavra-display { font-size: 2em; letter-spacing: 10px; margin: 30px 0; }
        .letras-erradas { color: red; }
        .teclado button { margin: 5px; padding: 10px; cursor: pointer; }
    </style>
</head>
<body>


<div id="app">
    <h1>Jogo da Forca</h1>


    <div class="palavra-display">
        {{ palavraAdivinhada }}
    </div>


    <p v-if="errosMaximos">Você perdeu! A palavra era: <strong>{{ palavraSecreta }}</strong></p>
    <p v-else-if="vitoria">Parabéns! Você venceu!</p>
    <p v-else>
        Tentativas restantes: <strong>{{ tentativasRestantes }}</strong>
    </p>


    <div v-if="!jogoAcabou" class="teclado">
        <button
            v-for="letra in alfabeto"
            :key="letra"
            @click="adivinhar(letra)"
            :disabled="letrasTentadas.includes(letra)">
            {{ letra }}
        </button>
    </div>


    <p>Letras erradas:
        <span class="letras-erradas">{{ letrasErradas.join(', ') }}</span>
    </p>
   
    <button @click="iniciarJogo">Reiniciar Jogo</button>


</div>


<script>
    const ForcaApp = {
        data() {
            return {
                palavras: ['Ponto e Virgula', 'informatica', 'CTI', 'ecommerce', 'Caderno'],
                palavraSecreta: '',
                letrasTentadas: [],
                maxErros: 6,
                alfabeto: Array.from('ABCDEFGHIJKLMNOPQRSTUVWXYZ') // Cria um array de A a Z
            };
        },


        // Propriedades Computadas (reativas, atualizam automaticamente)
        computed: {
            // Conta quantos erros foram cometidos
            erros() {
                return this.letrasTentadas.filter(letra =>
                    !this.palavraSecreta.includes(letra)
                ).length;
            },
           
            // Calcula quantas tentativas restam
            tentativasRestantes() {
                return this.maxErros - this.erros;
            },


            // Verifica se o máximo de erros foi atingido
            errosMaximos() {
                return this.erros >= this.maxErros;
            },


            // Cria a string para exibição (ex: V _ E _ S)
            palavraAdivinhada() {
                return this.palavraSecreta.split('').map(letra => {
                    return this.letrasTentadas.includes(letra) ? letra : '_';
                }).join(' ');
            },


            // Verifica se a palavra foi completamente adivinhada
            vitoria() {
                // Se a palavra adivinhada não contém mais '_', significa vitória
                return !this.palavraAdivinhada.includes('_') && this.palavraSecreta.length > 0;
            },


            // Verifica se o jogo deve parar (ganhou ou perdeu)
            jogoAcabou() {
                return this.errosMaximos || this.vitoria;
            },
           
            // Filtra e retorna apenas as letras que estão erradas
            letrasErradas() {
                return this.letrasTentadas.filter(letra =>
                    !this.palavraSecreta.includes(letra)
                );
            }
        },


        // Métodos (funções que a aplicação pode executar)
        methods: {
            // Seleciona uma palavra aleatória para começar
            iniciarJogo() {
                const indice = Math.floor(Math.random() * this.palavras.length);
                this.palavraSecreta = this.palavras[indice];
                this.letrasTentadas = []; // Reseta as tentativas
            },


            // Lógica principal de adivinhação
            adivinhar(letra) {
                // Apenas tenta se a letra ainda não foi usada e o jogo não acabou
                if (!this.letrasTentadas.includes(letra) && !this.jogoAcabou) {
                    this.letrasTentadas.push(letra);
                }
            }
        },
       
        // Hook de Ciclo de Vida: Chamado quando o componente (a app) é montado
        mounted() {
            this.iniciarJogo();
        }
    };


    Vue.createApp(ForcaApp).mount('#app');
</script>


</body>
</html>



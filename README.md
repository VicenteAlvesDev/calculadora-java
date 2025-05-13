# calculadora-java
Passo a passo para iniciar uma calculadora no NetBeans utilizando o método MVC (model view controller).


📦 ESTRUTURA DE PACOTES MVC

Você deve criar os seguintes pacotes:

- view → Interfaces gráficas (ex: CalculadoraView.java, com botões e campos)
- controller → Regras de funcionamento, controle dos botões e operações
- model → Lógica pura e dados, como métodos somar(), subtrair(), etc.

────────────────────────────────────────────
✅ COMO CRIAR OS PACOTES NO NETBEANS

1. Clique com o botão direito na pasta "Source Packages"
2. Vá em: New > Java Package
3. Nomeie como:
   - view
   - controller
   - model

Exemplo da estrutura final:

CalculadoraMVC  
 └── Source Packages  
      ├── controller  
      ├── model  
      └── view

────────────────────────────────────────────
🧱 CRIANDO A INTERFACE (VIEW)

1. Clique com o botão direito no pacote `calculadora.view`  
   ➤ New > JFrame Form  
   ➤ Nome: CalculadoraView  
   ➤ Clique em Finish

2. Monte a interface visual da calculadora usando a paleta:

Componentes recomendados:
- label → nomeie como `txtDisplay`
- Botões (JButton):
    - Números: 0 a 9
    - Operações: +, -, *, /
    - Igual: =
    - Limpar: C

3. Nomeie os botões corretamente.

---

🟢 MÉTODO ADICIONAR NÚMERO

Dentro da classe `CalculadoraView`, adicione:

public void adicionarNumero(String numero) {
    // código que atualiza o visor
}

Em cada botão de número (ex: btn1):

private void btn1ActionPerformed(java.awt.event.ActionEvent evt) {
    adicionarNumero("1");
}

OBS:  Você deve criar o método adicionarNumero dentro da classe CalculadoraView, mas fora do construtor.

🔹 Sim, dentro de:
public class CalculadoraView extends javax.swing.JFrame {
    // aqui dentro ✅
}

🔸 Mas não dentro de:
public CalculadoraView() {
    // aqui não ❌ (isso é o construtor)
}

---
🎯 IMPLEMENTANDO O MVC

📌 Crie duas classes:
1. CalculadoraModel → no pacote calculadora.model
2. CalculadoraController → no pacote calculadora.controller

Na CalculadoraView, vamos:
	- Instanciar o Controller
	- Chamar métodos dele quando os botões forem clicados

---

📦 CalculadoraModel.java (modelo básico):

package calculadora.model;

public class CalculadoraModel {
    private double resultado;

    public void adicionar(double valor) {
        resultado += valor;
    }

    public void subtrair(double valor) {
        resultado -= valor;
    }

    public void multiplicar(double valor) {
        resultado *= valor;
    }

    public void dividir(double valor) {
        if (valor != 0) {
            resultado /= valor;
        } else {
            throw new ArithmeticException("Divisão por zero");
        }
    }

    public void limpar() {
        resultado = 0;
    }

    public double getResultado() {
        return resultado;
    }
}

Esse será o cérebro da calculadora, responsável pelos cálculos.

---

📦 CalculadoraController.java (versão simplificada):

🧠 Imagina assim:
A View é o que o usuário vê: os botões, visor, etc.
O Model é quem faz as contas (soma, subtração, etc.)
O Controller é quem recebe os comandos da View e fala pro Model o que fazer.

📦 Exemplo prático:
O usuário digita 7
Depois clica em +
Depois digita 3
Depois clica em =

✅ Objetivo:
Vamos fazer a calculadora funcionar assim:

O usuário digita um número (ex: 7)
Clica no botão +
Digita outro número (ex: 3)
Clica no botão =

O visor mostra 10

package calculadora.controller;

public class CalculadoraController {
    private double primeiroNumero;
    private String operacao;

    public void salvarOperacao(String operacao, double valor) {
        this.primeiroNumero = valor;
        this.operacao = operacao;
    }

    public double calcular(double segundoNumero) {
	      if (operacao === "+") {
    		  return primeiroNumero + segundoNumero;
      	} else if (operacao === "-") {
          return primeiroNumero - segundoNumero;
      	} else {
          return segundoNumero;
      	}

    }

    public void limpar() {
        primeiroNumero = 0;
        operacao = "";
    }
}

🧠 Como funciona:
salvarOperacao("+", 7) → guarda o valor 7 e a operação +
calcular(3) → faz 7 + 3 e retorna 10
limpar() → reseta os valores

---

📌 INTEGRANDO VIEW E CONTROLLER
Perfeito! Agora vamos fazer a View conversar com o controller simplificado que criamos — ou seja, quando o usuário clicar no botão +, vamos:

Pegar o número que estava no visor
Enviar para o controller.salvarOperacao("+", valor)
Limpar o visor para ele digitar o próximo número

No topo da classe CalculadoraView, importe o controller:
import controller.CalculadoraController;

🟢 1. Verifique se você já tem essas variáveis no topo da sua classe:
private CalculadoraController controller;
private String textoAtual = "";

Se ainda não estiver, coloque.

🟢 2. No construtor da View, instancie o controller:
public CalculadoraView() {
    initComponents();
    controller = new CalculadoraController();
}

🟢 3. Vá até o botão + e adicione este código no método dele (btnMaisActionPerformed):
double valor = Double.parseDouble(txtDisplay.getText());
controller.salvarOperacao("+", valor);
textoAtual = "";
txtDisplay.setText("");

🟢 4. Agora vá até o botão = e coloque isso no btnIgualActionPerformed:
double segundoNumero = Double.parseDouble(txtDisplay.getText());
double resultado = controller.calcular(segundoNumero);
txtDisplay.setText(String.valueOf(resultado));
textoAtual = String.valueOf(resultado); // se o usuário continuar digitando

✅ Testando:
Clique nos números: ex. 7
Clique em +
Clique nos números: ex. 3
Clique em =
Resultado 10 aparece no visor 🎉

---

🔁 BOTÃO "LIMPAR"

No botão btnLimpar:

private void btnLimparActionPerformed(java.awt.event.ActionEvent evt) {
    textoAtual = "";
    txtDisplay.setText("");
    controller.resetar();
}

✅ Resultado:
Quando o usuário clicar no botão "Limpar", tudo zera e ele pode começar de novo.

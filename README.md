# Sistema de Saúde e Bem-Estar — Correção de Bugs
 
Documentação dos bugs encontrados e corrigidos no sistema de console para cálculo de IMC, recomendação de água e frequência cardíaca máxima.
 
## Bug 1 — Multiplicação em vez de potenciação no cálculo do IMC
 
**Problema:** o cálculo usava `peso / altura * altura`, uma cadeia de multiplicação/divisão que não equivale a `peso / altura²`.
 
**Correção:**
```python
imc = peso / (altura ** 2)
```
 
## Bug 2 — Faixas de classificação sobrepostas e sem retorno para valores-limite
 
**Problema:** o uso de operadores estritos (`>` e `<`) deixava os valores exatos (18.5, 25.0, 30.0) e o intervalo 24.9–25.0 fora de qualquer faixa, retornando `None`.
 
**Correção:**
```python
def classificar_imc(imc):
    if imc < 18.5:
        return "Abaixo do peso"
    elif imc < 25.0:
        return "Peso normal"
    elif imc < 30.0:
        return "Sobrepeso"
    else:
        return "Obesidade"
```
 
## Bug 3 — Fórmula de água diária dividindo em vez de multiplicar
 
**Problema:** a fórmula usava `peso / 35` em vez de `peso * 35ml`, invertendo a lógica — quanto maior o peso, menor a recomendação.
 
**Correção:**
```python
def calcular_agua_diaria(peso):
    ml = peso * 35
    litros = ml / 1000
    return litros
```
 
## Bug 4 — Frequência cardíaca máxima somando em vez de subtrair
 
**Problema:** a fórmula somava a idade a 220 (`220 + idade`) em vez de subtrair, invertendo a lógica — quanto maior a idade, maior a FC máxima, quando deveria diminuir.
 
**Correção (Fórmula de Haskell/Fox):**
```python
def calcular_frequencia_cardiaca_maxima(idade):
    fc_max = 220 - idade
    return fc_max
```
 
## Bug 5 — `input()` retorna string sem conversão no menu
 
**Problema:** `input()` sempre retorna string, mas o restante do código comparava a opção escolhida com valores inteiros, então as comparações nunca eram verdadeiras.
 
**Correção:**
```python
def menu():
    # ... exibição do menu ...
    opcao = input("Escolha uma opção (1-4): ")
    try:
        return int(opcao)
    except ValueError:
        print("Opção inválida! Digite um número entre 1 e 4.")
        return menu()
```
 
## Bug 7 — Ausência de `break` para sair do loop infinito
 
**Problema:** ao escolher a opção 4 (Sair), o programa exibia a mensagem de despedida mas não interrompia o `while True`, fazendo o menu reaparecer indefinidamente.
 
**Correção:**
```python
elif opcao == 4:
    print("Encerrando o sistema...")
    print("Obrigado por usar nosso sistema!")
    break
```
 
> **Observação:** as comparações `opcao == 1`, `opcao == 2` etc. (incluindo a do Bug 7) só funcionam corretamente depois que o Bug 5 é corrigido, já que dependem de `menu()` retornar um `int`.
 

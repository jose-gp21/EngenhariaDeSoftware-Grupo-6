# README - Aplicativo da Seguradora de Veículos

## Equipe

- **Integrante 1:** [Nome do Integrante 1]
- **Integrante 2:** [Nome do Integrante 2]
- **Integrante 3:** [Nome do Integrante 3]
- **Integrante 4:** [Nome do Integrante 4]

## Descrição do Problema

Após um acidente com o veículo do segurado, o processo de acionar a seguradora envolve as seguintes etapas:

1. **Boletim de Ocorrência:**
   - O segurado aciona a polícia para emitir o boletim de ocorrência.
   - Com o boletim em mãos, o segurado fotografa e notifica a seguradora.

2. **Transporte do Veículo:**
   - Se o veículo estiver em condições de trafegar, o segurado o levará até a oficina.
   - Se o veículo não estiver em condições de trafegar, a seguradora acionará um agente rebocador para transportar o veículo até a oficina para avaliação dos danos.

3. **Avaliação dos Danos:**
   - Se a oficina detectar perda total, a seguradora depositará o valor segurado (prêmio).
   - Caso contrário, a oficina consertará o veículo, e a seguradora cobrará a franquia, que o segurado deverá pagar.

4. **Emissão de Boleto para Franquia:**
   - O segurado deve ter a opção no aplicativo da seguradora para emitir um boleto (código de barras ou QR Code) referente à franquia a ser paga.

## Estrutura do Código

Abaixo está um exemplo básico de como o aplicativo da seguradora pode ser estruturado utilizando o framework Flask para uma aplicação web simples.

```python
# app.py
from flask import Flask, request, jsonify, send_file
import random
import string

app = Flask(__name__)

# Função para gerar códigos de boletos
def gerar_codigo_boleto():
    return ''.join(random.choices(string.ascii_uppercase + string.digits, k=12))

# Rota para notificar seguradora com o boletim de ocorrência
@app.route('/notificar', methods=['POST'])
def notificar():
    data = request.json
    boletim = data.get('boletim')
    if boletim:
        # Salvar dados e notificar a seguradora
        # Aqui você pode adicionar lógica para salvar dados em um banco de dados
        return jsonify({"message": f"Boletim de Ocorrência {boletim} registrado."}), 200
    return jsonify({"message": "Boletim de Ocorrência não fornecido."}), 400

# Rota para gerar e emitir boleto
@app.route('/emitir_boleto', methods=['POST'])
def emitir_boleto():
    data = request.json
    valor_franquia = data.get('valor_franquia')
    if valor_franquia:
        codigo_boleto = gerar_codigo_boleto()
        # Aqui você pode adicionar lógica para gerar e salvar o boleto
        return jsonify({"boleto_codigo": codigo_boleto, "valor_franquia": valor_franquia}), 200
    return jsonify({"message": "Valor da franquia não fornecido."}), 400

if __name__ == '__main__':
    app.run(debug=True)

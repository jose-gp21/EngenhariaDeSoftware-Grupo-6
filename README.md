# README - Aplicativo da Seguradora de Veículos

## Equipe

- **Integrante 1:** José Gabriel dos Santos Paludo
- **Integrante 2:** Gustavo Brunoni

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

### Código Fonte

O código está no arquivo `seguradora.cpp`. Abaixo está um resumo das principais funcionalidades:

```cpp
#include <iostream>
#include <fstream>
#include <string>
#include <cstdlib>
#include <ctime>

class AplicativoSeguradora {
public:
    void registrarBoletim(const std::string& boletim);
    void emitirBoleto(double valorFranquia);

private:
    std::string gerarCodigoBoleto();
};

void AplicativoSeguradora::registrarBoletim(const std::string& boletim) {
    std::ofstream arquivo("boletim.txt");
    if (arquivo.is_open()) {
        arquivo << "Boletim de Ocorrência registrado: " << boletim << std::endl;
        arquivo.close();
        std::cout << "Boletim de Ocorrência " << boletim << " registrado com sucesso." << std::endl;
    } else {
        std::cerr << "Erro ao abrir o arquivo para registrar o boletim." << std::endl;
    }
}

void AplicativoSeguradora::emitirBoleto(double valorFranquia) {
    std::string codigoBoleto = gerarCodigoBoleto();
    std::ofstream arquivo("boleto.txt");
    if (arquivo.is_open()) {
        arquivo << "Código do Boleto: " << codigoBoleto << std::endl;
        arquivo << "Valor da Franquia: R$ " << valorFranquia << std::endl;
        arquivo.close();
        std::cout << "Boleto emitido com sucesso." << std::endl;
    } else {
        std::cerr << "Erro ao abrir o arquivo para emitir o boleto." << std::endl;
    }
}

std::string AplicativoSeguradora::gerarCodigoBoleto() {
    const std::string caracteres = "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
    std::string codigo;
    srand(static_cast<unsigned>(time(0))); // Inicializa o gerador de números aleatórios

    for (int i = 0; i < 12; ++i) {
        codigo += caracteres[rand() % caracteres.length()];
    }

    return codigo;
}

int main() {
    AplicativoSeguradora seguradora;
    std::string boletim;
    double valorFranquia;

    // Entrada de dados do usuário
    std::cout << "Digite o número do boletim de ocorrência: ";
    std::getline(std::cin, boletim);
    seguradora.registrarBoletim(boletim);

    std::cout << "Digite o valor da franquia: R$ ";
    std::cin >> valorFranquia;
    seguradora.emitirBoleto(valorFranquia);

    return 0;
}

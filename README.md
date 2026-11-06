# Gerenciador de Manutenção de Equipamentos (Projeto 1 Estrutura de Dados)

Sistema em **C** para organizar a fila de manutenção dos equipamentos de um laboratório, construído inteiramente sobre uma **lista simplesmente encadeada** com alocação dinâmica de memória.

Projeto desenvolvido para a disciplina **Estrutura de Dados (Prática)** do curso de Engenharia de Computação da **PUC-Campinas** (1º Projeto Avaliativo – 2026).

---

## Sobre o projeto

O Laboratório de Sistemas Embarcados e Automação controlava os pedidos de manutenção (osciloscópios, fontes, multímetros, impressoras 3D etc.) em uma planilha. Este sistema substitui esse controle manual por um programa que permite cadastrar, remover, consultar e alterar solicitações, e mostra a **ordem em que os equipamentos devem ser consertados**, de acordo com a urgência.

Restrições seguidas no projeto:

- uso obrigatório de **lista simplesmente encadeada**;
- **sem** vetores para armazenar ou ordenar as solicitações;
- **sem** funções prontas de ordenação;
- **sem** variáveis globais;
- toda a memória alocada é liberada antes de o programa encerrar.

## Funcionalidades

| Opção | Funcionalidade | Como funciona |
| --- | --- | --- |
| 1 | Inserir solicitação | Insere já na posição correta, mantendo a lista ordenada pelo código. Recusa códigos repetidos. |
| 2 | Remover solicitação | Remove pelo código e libera a memória do nó. |
| 3 | Consultar solicitação | Mostra todos os dados de uma solicitação. |
| 4 | Alterar prioridade/período | Atualiza os valores, respeitando o limite de dias da nova prioridade. |
| 5 | Exibir ordem de manutenção | Monta uma lista temporária ordenada por urgência, sem alterar a lista principal. |
| 6 | Exibir todas as solicitações | Lista tudo em ordem de código. |
| 0 | Sair | Libera toda a memória e encerra. |

### Ordem de manutenção (opção 5)

As solicitações são atendidas seguindo três critérios, nesta ordem:

1. **Prioridade** – 1 (Alta), depois 2 (Média), depois 3 (Baixa);
2. **Período** – com a mesma prioridade, vai antes quem pode ficar menos dias parado;
3. **Código** – com prioridade e período iguais, vai antes o menor código.

## Dados de cada solicitação

| Campo | Regra |
| --- | --- |
| Código da solicitação | Inteiro de 4 dígitos (1000 a 9999), único |
| Código do equipamento | 3 letras + 3 dígitos (ex.: `OSC023`), convertido para maiúsculas |
| Nome do equipamento | Até 20 caracteres, não pode ser vazio |
| Prioridade | 1 = Alta, 2 = Média, 3 = Baixa |
| Período (dias) | Prioridade 1: 1 a 7 · Prioridade 2: 1 a 15 · Prioridade 3: 1 a 20 |

Todas as entradas são validadas: se o usuário digitar um valor inválido (inclusive letras em campos numéricos), o programa pede o dado de novo.

## Estrutura do projeto

```
├── main.c                  # Menu e funções de leitura/validação da entrada
├── Biblioteca_Projeto1.h   # Estruturas e todas as operações da lista encadeada
└── README.md
```

### Estruturas de dados

```c
typedef struct {
    int  codigoSolicitacao;
    char codigoEquipamento[10];
    char nomeEquipamento[21];
    int  prioridade;
    int  periodo;
} Manutencao;            // dados de uma solicitação

typedef struct No {
    Manutencao info;
    struct No *prox;
} No;                    // nó da lista encadeada

typedef struct {
    No *inicio;
} Lista;                 // descritor da lista
```

### Funções da biblioteca

| Função | Descrição |
| --- | --- |
| `CriaLista` | Aloca uma lista vazia |
| `inserir` | Inserção ordenada por código, com verificação de duplicidade |
| `remover` | Remove um nó pelo código e libera a memória |
| `consultar` | Busca um nó pelo código e exibe os dados |
| `alterar` | Altera prioridade e período com validação |
| `ImprimeLista` | Exibe todos os nós de uma lista |
|

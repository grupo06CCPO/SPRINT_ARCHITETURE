# SPRINT_ARCHITETURE
 ChargeCore
Módulo de Autenticação RFID em Assembly RISC-V para Eletropostos Comerciais
EV Challenge 2026 — FIAP + GoodWe | Arquitetura de Computadores | Sprint 1

 Integrantes
Nome
RM
Ana Julia Yumi
569430
Maria Fernanda
569999
Julia Nunes
569858
Rafael Rebello
570642


 Problema
Eletropostos comerciais geralmente utilizam software escrito em linguagens de alto ou médio nível (como C++ ou Java) rodando em hardware genérico. Isso gera:
Consumo desnecessário de energia computacional
Instruções extras que desperdiçam ciclos de clock
Baixa eficiência no processamento de operações críticas como autenticação de usuários
Em ambientes com múltiplos carregadores simultâneos, esse desperdício se multiplica

Justificativa
A escolha da linguagem e da arquitetura de processador tem impacto direto no consumo energético de um sistema embarcado. Em um eletroposto que opera 24 horas por dia, 7 dias por semana, mesmo pequenas reduções no consumo computacional resultam em ganhos reais de eficiência e sustentabilidade.
A arquitetura RISC-V foi escolhida por ser:
Open source e amplamente utilizada em sistemas embarcados
Baseada em conjunto reduzido de instruções (RISC)
Capaz de executar cada instrução em um único ciclo de clock
Ideal para dispositivos de baixo consumo energético

 Proposta de Solução
O Charge Core é um módulo de controle para autenticação de usuários em eletropostos, escrito em Assembly RISC-V. Ele substitui a camada de autenticação de alto nível por código de baixo nível, eliminando o overhead de compiladores e reduzindo o número de instruções executadas.
Fluxo do sistema:
Usuário → Cartão RFID → Leitura do ID → Validação → Liberação da recarga


 Arquitetura Utilizada
Arquitetura: RISC-V (Reduced Instruction Set Computer)
Simulador: RARS (RISC-V Assembler and Runtime Simulator)
Paradigma: Programação em Assembly de baixo nível
Conceitos aplicados:
Ciclos de clock por instrução
Registradores de uso geral
Instruções de comparação e desvio condicional
Chamadas de sistema (syscall)

Código Assembly
Trecho simulado no RARS representando a lógica de autenticação RFID:
# ChargeCore - Autenticação RFID em RISC-V Assembly
# Simulado no RARS | FIAP + GoodWe | Sprint 1

        .data
id_autorizado:  .word 4829          # ID do cartão RFID autorizado
msg_ok:         .string "Acesso liberado. Iniciando recarga...\n"
msg_negado:     .string "Acesso negado. Cartao nao autorizado.\n"

        .text
        .globl main

main:
        # Carrega o ID lido pelo sensor RFID (simulado como valor fixo)
        li      t0, 4829            # t0 = ID lido pelo leitor

        # Carrega o ID autorizado da memória
        la      t1, id_autorizado   # t1 = endereço do ID autorizado
        lw      t2, 0(t1)           # t2 = valor do ID autorizado

        # Compara os dois IDs
        bne     t0, t2, negado      # se forem diferentes, vai para "negado"

        # Acesso autorizado
        li      a7, 4               # syscall 4 = imprimir string
        la      a0, msg_ok
        ecall
        j       fim

negado:
        # Acesso negado
        li      a7, 4
        la      a0, msg_negado
        ecall

fim:
        li      a7, 10              # syscall 10 = encerrar programa
        ecall


 Impactos Esperados
Métrica
Alto Nível (C/Java)
Assembly RISC-V (ChargeCore)
Instruções por autenticação
~80–120
~15–20
Ciclos de CPU utilizados
Alto
Reduzido
Consumo energético
Maior
Menor
Dependência de runtime
Sim
Não


Relação com Sustentabilidade e Energias Renováveis
Menos instruções = menos ciclos de CPU = menos energia consumida pelo processador do eletroposto
Em uma rede com dezenas de eletropostos operando 24h, a economia computacional acumulada é significativa
Processadores mais eficientes permitem o uso de hardware de menor consumo, alimentado por energia solar (como os inversores GoodWe)
O Charge Core está alinhado com a visão do EV Challenge: transformar cada sessão de recarga em um processo mais inteligente e sustentável

Próximos Passos
Expandir o módulo com controle de sessão de recarga
Estruturar dados de consumo em registradores
Integração conceitual com o backend da GoodWe
Comparação quantitativa de consumo: C vs Assembly (próxima sprint)

A mobilidade elétrica do futuro não depende apenas da capacidade das baterias. Depende do código que orquestra e distribui essa energia. — EV Challenge 2026, FIAP + GoodWe

# Implementação 4 — Pint-OS Alarm Clock

Infraestrutura de Software (CESAR School). Reimplementação de `timer_sleep()`
no Pint-OS sem busy wait, usando bloqueio de thread e uma fila ordenada de
threads dormindo.

## Estrutura

Este repositório contém só o que o enunciado pede para a entrega, não a árvore
completa do Pint-OS (que já é fornecida pelo professor):

```
lagi/
├── threads/thread.h   campo wakeup_ticks adicionado a struct thread
├── devices/timer.c    timer_sleep, timer_interrupt e wakeup_less reescritos
├── Makefile           Makefile original do projeto threads/ do Pint-OS
└── evidencias.log     comandos e saídas reais de build e teste
```

Para compilar, os dois arquivos de `lagi/threads` e `lagi/devices` devem ser
colocados nos respectivos diretórios de uma árvore completa do Pint-OS, e o
build é feito de dentro de `threads/` com `make`.

## O que foi feito

- `struct thread` ganhou o campo `wakeup_ticks`, reaproveitando o
  `list_elem` já existente (`elem`) para colocar a thread numa lista de
  espera enquanto está bloqueada.
- `timer_sleep()` não faz mais polling: calcula o tick de despertar, insere a
  thread atual ordenada numa lista (`sleeping_list`) e chama `thread_block()`.
- `timer_interrupt()` percorre o início dessa lista a cada tick e acorda
  (`thread_unblock()`) toda thread cujo horário já chegou.
- Interrupções só são desabilitadas para proteger o acesso à `sleeping_list`
  entre thread e handler de interrupção — a exceção de sincronização que o
  próprio enunciado permite.

## Testes

Os 5 testes de alarm clock do Pint-OS passam: `alarm-single`,
`alarm-multiple`, `alarm-simultaneous`, `alarm-zero`, `alarm-negative`.
`alarm-priority` falha, pois exige escalonamento por prioridade, fora do
escopo desta entrega (ver relatório para detalhes).

## Relatório

[link do PDF a preencher]

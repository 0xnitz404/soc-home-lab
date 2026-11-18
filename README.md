# Laboratório de SOC em casa

Um SOC pequeno rodando em localhost, com Hyper-V e WSL2: máquinas que geram
logs, um SIEM que recebe, ataques simulados para gerar alertas e a triagem de
cada alerta, como no dia a dia de um analista.

**Status:** Fase 0 em andamento (rede isolada e NAT criados no Hyper-V).

## Objetivo

Praticar de ponta a ponta o trabalho de um analista de SOC N1: coletar logs,
detectar, investigar, classificar pela MITRE ATT&CK e documentar.

## Arquitetura

Rede isolada `LabSOC` (10.10.10.0/24) no Hyper-V. As VMs saem para a internet
por NAT, e nada de fora enxerga as VMs. O Kali roda no WSL2, fora dessa rede,
e chega nas VMs pelo host.

| Máquina | Endereço | Papel | Fase |
|---|---|---|---|
| Host (gateway) | 10.10.10.1 | NAT | 0 |
| Ubuntu Server 24.04 | 10.10.10.10 | SIEM (Wazuh) | 0 |
| Windows 11 | 10.10.10.20 | Endpoint monitorado | 0 |
| Kali Linux (WSL2) | via host | Atacante | 3 |
| Windows Server | 10.10.10.5 | Controlador de domínio (AD) | 4 |

## Fases

| Fase | O que entra | Como sei que funcionou | Status |
|---|---|---|---|
| 0 | Rede do lab, VMs Ubuntu e Windows 11 | As VMs se enxergam e saem para a internet | Em andamento |
| 1 | Wazuh (SIEM + EDR), agente e Sysmon no Windows | Errar a senha 5 vezes e ver o alerta no painel | A fazer |
| 2 | Atomic Red Team | Ataque simulado vira alerta com técnica MITRE e ticket de triagem | A fazer |
| 3 | Suricata, Wireshark e Kali (WSL2) | Achar varredura e força bruta no tráfego e no SIEM | A fazer |
| 4 | Active Directory | Detectar criação de usuário suspeito no domínio | A fazer |
| 5 | Splunk | Refazer a investigação com consultas SPL | A fazer |
| 6 | Shuffle (SOAR) em Docker e scripts | Alerta que abre ticket sozinho | A fazer |

## Regras do laboratório

- Ferramentas de ataque rodam só no Kali (WSL2) e nas VMs do lab.
- Os alvos são só as máquinas da rede do lab.
- Checkpoint antes de cada fase.
- Nenhum dado real: tudo aqui é do laboratório.

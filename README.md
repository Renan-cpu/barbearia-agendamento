# Barbearia — Agendamentos com Aprovação e Sinal (50%)

Sistema completo para barbearias que hoje controlam agenda em caderno/WhatsApp.
A proposta é permitir que clientes solicitem horários pelo celular, o barbeiro aprove/rejeite e, quando aprovado, o horário seja bloqueado no calendário (1h por corte). Também suporta pagamento de sinal (50%) no agendamento.

## ✨ Funcionalidades (MVP)
### Cliente (App)
- Cadastro/Login
- Visualização de serviços (ex.: corte) e preços
- Consulta de disponibilidade (slots de 1 hora)
- Solicitação de agendamento (status **PENDENTE**)
- Acompanhamento do status do agendamento (pendente/aprovado/recusado)
- Pagamento de sinal **50%** (via integração com gateway e confirmação por webhook)

### Barbeiro/Admin (App)
- Lista de solicitações pendentes
- Aprovar/recusar agendamentos
- Visualizar agenda (dia/semana) com horários bloqueados
- Configurar horário de funcionamento
- Cadastrar folgas/exceções
- Ver status do pagamento do sinal

## 🧠 Regras de negócio
- **1h por corte** (slot fixo de 60 min)
- O agendamento nasce como **REQUESTED (PENDENTE)**
- Ao **aprovar**, o sistema **bloqueia o slot** para evitar dupla reserva
- Pagamento do sinal confirmado via **webhook** (idempotente)

> Nota de concorrência: o bloqueio do horário é garantido via persistência em banco usando estratégia de slots com restrição UNIQUE (ex.: `date + hour`), evitando duplo agendamento.

## 🏗️ Arquitetura (visão geral)
- **Mobile (Android)**: App para Cliente e Barbeiro/Admin (um app com perfis ou dois apps)
- **Backend**: API REST em Java
- **Banco**: PostgreSQL
- **Pagamentos**: integração com gateway + webhook
- **Disponibilidade**: regras de funcionamento + folgas + slots ocupados

## 🛠️ Tecnologias
### Backend
- Java 17+
- Spring Boot 3
- Spring Web (REST)
- Spring Data JPA
- Spring Security (JWT)
- PostgreSQL
- Flyway ou Liquibase (migrations)
- Docker / Docker Compose
- OpenAPI/Swagger

### Mobile
- Android (Java)
- Retrofit + OkHttp
- (Opcional) Firebase Cloud Messaging (push)

## 📦 Estrutura sugerida do repositório

backend/ # Spring Boot API
mobile/ # Android app (cliente + barbeiro/admin)
docs/ # diagramas, decisões de arquitetura, guias
docker/ # compose, scripts auxiliares


## 🚀 Como rodar localmente (planejado)
### Backend + Banco
1. Subir Postgres via Docker Compose
2. Subir a API (Spring Boot)
3. Acessar Swagger em: `http://localhost:8080/swagger-ui.html`

> Este guia será atualizado conforme o projeto evoluir.

## 🗺️ Roadmap (alto nível)
- [ ] Base do backend + migrations + Swagger
- [ ] Auth (JWT) + perfis CLIENT/BARBER
- [ ] Serviços (corte, preço, duração)
- [ ] Disponibilidade (horário/folgas) + slots de 1h
- [ ] Agendamentos (solicitar/aprovar/recusar) + bloqueio de horário
- [ ] Pagamento do sinal (50%) + webhook
- [ ] App Cliente (fluxo completo)
- [ ] App Barbeiro/Admin (aprovação + calendário)




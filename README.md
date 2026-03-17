# AI-agent-for-scheduling

Este projeto consiste em um workflow no n8n que implementa um assistente virtual inteligente via WhatsApp, capaz de:

Atender clientes automaticamente

Entender mensagens (texto e áudio)

Gerenciar leads

Realizar agendamentos integrados ao Google Calendar

Responder de forma humanizada usando IA

🚀 Funcionalidades
💬 Atendimento automatizado

Recebe mensagens via webhook (Evolution API / WhatsApp)

Identifica tipo de mensagem:

Texto

Áudio (com transcrição automática)

🧠 Inteligência Artificial

Utiliza modelo gpt-5-mini

Persona definida (Sofia – assistente de salão)

Fluxo estruturado de atendimento:

Saudação

Identificação da necessidade

Coleta de nome

Oferta de agendamento

Confirmação

Finalização

📅 Sistema de Agendamento

Integração com Google Calendar

Funcionalidades:

Consultar horários disponíveis

Criar agendamentos

Reagendar

Cancelar

🗃️ Gestão de Leads

Verifica se o cliente já existe (Supabase)

Cria novo lead automaticamente se necessário

Armazena:

Nome

Telefone

Data de criação

🧠 Memória de Conversa

Armazenamento em PostgreSQL

Mantém contexto das interações (últimas 20 mensagens)

🔄 Buffer de Mensagens

Usa Redis para:

Agrupar mensagens enviadas rapidamente (ex: várias mensagens seguidas)

Evitar respostas fragmentadas

🔊 Suporte a Áudio

Converte áudio (base64 → arquivo)

Transcreve usando OpenAI

Trata como texto no fluxo

🧩 Estrutura do Workflow
1. Entrada de Dados

Webhook EVO

Recebe mensagens do WhatsApp

2. Tratamento Inicial

Node Dados

Extrai:

Nome

Mensagem

Número

3. Verificação de Lead

Consulta no Supabase

Se não existir → cria novo lead

4. Roteamento de Mensagem

Switch

Texto

Áudio

5. Processamento de Áudio

Conversão para arquivo

Transcrição

6. Buffer (Redis)

Armazena mensagens temporariamente

Aguarda (Wait)

Junta mensagens em uma única entrada

7. IA (Coração do Sistema)

AI Agent

Usa prompt estruturado

Controla fluxo de atendimento

Ferramentas disponíveis:

agendamentos (MCP)

Google Calendar

8. Pós-processamento

Divide mensagens longas

Envia em partes (simulando humano)

9. Envio de Resposta

Envio via Evolution API

🛠️ Tecnologias Utilizadas

n8n

OpenAI (GPT-5-mini)

Evolution API (WhatsApp)

Redis (buffer de mensagens)

PostgreSQL (memória)

Supabase (leads)

Google Calendar API (agendamentos)

⚙️ Configuração Necessária

Antes de usar o workflow, configure:

🔑 Credenciais

OpenAI API

Supabase

Redis

PostgreSQL

Google Calendar OAuth

Evolution API

🗄️ Banco de Dados

Tabela usada:

create table dados_cliente (
  id bigserial primary key,
  created_at TIMESTAMPTZ,
  telefone text,
  nomewpp text
);
📌 Regras Importantes do Agente

Máximo de 2–3 linhas por mensagem

Sempre uma pergunta por vez

Nunca agendar sem confirmação

Sempre verificar disponibilidade antes

Respeitar horário:

🕗 08h às 18h

📅 Segunda a sexta

🔄 Fluxo de Agendamento

Identifica interesse

Coleta nome

Pergunta dia

Verifica disponibilidade

Mostra horários

Recebe escolha

Confirma dados

Agenda no sistema

Envia confirmação final

📤 Formato de Saída

O agente retorna sempre em JSON:

{
  "mensagem": "texto para o cliente",
  "event_id": "id_do_evento",
  "patient_name": "Nome",
  "dateTime": "2025-08-04T08:00:00-03:00"
}
🧠 Diferenciais

Atendimento humanizado (parece pessoa real)

Suporte a áudio e texto

Buffer inteligente (evita respostas quebradas)

Integração completa com agenda

Estrutura pronta para escalar

📈 Possíveis Melhorias

Integração com CRM

Dashboard de agendamentos

Multi-atendentes

Suporte a múltiplos serviços simultâneos

Notificações automáticas (lembrete de consulta)

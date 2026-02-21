# Evolução incremental — roadmap comercial, conversão e retenção

## 1) Plano por fases (quick wins vs médio prazo)

### Fase 0 (quick wins: 1–2 sprints)
- Uploads com progresso reutilizável em fluxos críticos e prevenção de duplo submit.
- Rejeição de faturas com motivo obrigatório e auditoria com timestamp.
- Gestão de utilizadores com anonimização e eliminação segura por dependências.
- Endpoints administrativos de operação: notificações internas, SLA e conversão afiliados CSV.
- Governança UTM inicial e tracking de atribuição por etapa de funil.

### Fase 1 (médio prazo: 2–4 sprints)
- Landing pages dinâmicas por persona com blocos de prova social, CTA e argumentos específicos.
- Motor de ofertas com cupões por campanha, descontos temporais, first-order bonus e bundles.
- Lead scoring + automação de nutrição (email/WhatsApp/notificações) com reengajamento.
- A/B test framework leve para headline/CTA/preço âncora/prova social.
- Smart links de afiliados com campanha/canal/oferta.

### Fase 2 (escala comercial: 4–8 sprints)
- Comissão multinível, atribuição híbrida first-click/last-click com janela configurável.
- Anti-fraude afiliado e revisão automática.
- Checkout 1-page, recuperação de pedido, referral pós-compra e NPS/CSAT acionável.
- BI completo: CAC, ROAS, LTV, conversão por canal com endpoints de exportação.

## 2) Lista de ficheiros alterados/criados
- `php-app/public/upload-utils.js`
- `php-app/public/styles.css`
- `php-app/public/main.js`
- `php-app/public/invoice.js`
- `php-app/public/admin.js`
- `php-app/public/admin-chat.js`
- `php-app/public/chat-widget.js`
- `php-app/public/order.html`
- `php-app/public/invoice.html`
- `php-app/public/admin.html`
- `php-app/public/admin-chat.html`
- `php-app/public/index.html`
- `php-app/public/services.html`
- `php-app/public/documents.html`
- `php-app/src/controllers/AdminController.php`
- `php-app/src/controllers/MarketingController.php`
- `php-app/src/models/User.php`
- `php-app/src/routes/api.php`
- `docs/evolucao_incremental.md`

## 3) Endpoints criados/alterados

### Marketing e aquisição
- `GET /api/marketing/landing-config?persona=`
- `GET /api/marketing/offers?campaign=`
- `GET /api/marketing/ab/assign?test=&visitor_id=`
- `POST /api/marketing/attribution`
- `GET /api/marketing/funnel`
- `POST /api/affiliates/smart-link`
- `GET /api/marketing/attribution/model`
- `POST /api/marketing/attribution/model`
- `POST /api/marketing/recovery`
- `POST /api/marketing/referral/post-purchase`
- `POST /api/marketing/nps`

### Admin e BI
- `GET /api/admin/notifications-center`
- `GET /api/admin/affiliates/conversion.csv`
- `GET /api/admin/sla`
- `GET /api/admin/growth-dashboard`
- `GET /api/admin/affiliates/fraud`
- `GET /api/admin/affiliates/campaigns`
- `POST /api/admin/affiliates/campaigns/save`
- `GET /api/admin/affiliates/materials`
- `POST /api/admin/affiliates/materials/save`
- `GET /api/admin/affiliates/control`
- `POST /api/admin/affiliates/settings/save`
- `POST /api/admin/affiliates/materials/delete`
- `POST /api/admin/affiliates/campaigns/delete`
- `POST /api/admin/users/anonymize`
- `GET /api/admin/marketing/recipients`
- `POST /api/admin/marketing/campaign/send`
- `GET /api/admin/marketing/campaign/history`

### Alterados
- `POST /api/admin/invoices/reject` exige `reason`, muda estado para `REJEITADA` e audita motivo.
- `POST /api/admin/users/delete` bloqueia hard delete quando há dependências (`409` + resumo).

## 4) Passos de validação manual por fluxo
1. **Encomendas com materiais**
   - anexar ficheiros e submeter;
   - validar barra de progresso e botão desativado;
   - confirmar redirecionamento para fatura.
2. **Serviços com anexo**
   - submeter com attachment;
   - validar progresso e prevenção de clique duplo.
3. **Comprovativo de pagamento**
   - enviar comprovativo em `invoice.html`;
   - verificar progresso e refresh de estado.
4. **Upload final admin**
   - enviar ficheiro final;
   - validar progresso e atualização da lista.
5. **Chat admin + fallback e chat suporte**
   - enviar mensagem com anexo;
   - validar progresso, envio único e refresh.
6. **Faturas rejeitadas**
   - testar erro sem motivo;
   - testar sucesso com motivo e auditoria.
7. **Utilizadores**
   - anonimizar utilizador;
   - tentar eliminar utilizador com dependências e validar bloqueio.
8. **Marketing/acquisition**
   - testar landing por persona, oferta por campanha, AB assign e tracking UTM;
   - consultar funil e growth dashboard.

## 5) Métricas de impacto esperado
- **Upload reliability**: redução de 25–40% de retrabalho por duplicação/abandono.
- **Conversão**: ganho incremental de 5–12% com oferta + persona + AB tests.
- **Velocidade comercial**: melhor priorização com lead scoring (hot/warm/cold).
- **Operação/BI**: visibilidade contínua de CAC/ROAS/LTV e conversão por canal.
- **Risco e compliance**: menor exposição de dados com anonimização e auditoria reforçada.


## 6) Incrementos adicionais nesta iteração
- Painel de crescimento no admin com KPIs comerciais estimados e breakdown por canal.
- Painel anti-fraude afiliados com bursts de clique, auto-referência e recomendações de bloqueio.
- Configuração de modelo de atribuição first-click/last-click com janela configurável.
- Endpoint de recuperação de checkout/pedido abandonado com cadência inicial.
- Referral pós-compra e NPS/CSAT acionável com gatilhos de upsell/depoimento/suporte.

- Painel admin para envio automático/manual de emails promocionais em massa usando credenciais PHPMailer do .env.

- Campanhas promocionais com envio de teste, deduplicação de destinatários e histórico no painel admin.

- Painel de afiliados profissional (admin e cliente) com campanhas, biblioteca de materiais, leaderboard e controlo operacional.

- Admin passou a gerir cada elemento de afiliados (edição/remoção de campanhas e materiais + configuração global de comissões/atribuição).

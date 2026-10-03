# LiDire MVP 5.2 — IA LiDire

Atualização da versão 4.4 com suporte funcional a:
- idioma Português (Brasil) e English;
- aparência somente Claro ou Escuro;
- preferência persistida em localStorage;
- versão clara preservando a paleta oficial da LiDire (roxo/ciano);
- manutenção das demais funcionalidades da 4.4.

## Atualização 4.5 — Família, convite e acabamento visual

A versão 4.5 mantém a identidade visual oficial da LiDire e acrescenta:
- refinamento do modo claro, com contraste consistente em campos, cards, textos e navegação;
- permissões familiares reorganizadas em linhas com checkbox + ícone + nome do recurso;
- prévia visual do convite familiar usando exclusivamente `logo-lidire-oficial.png`;
- fluxo separado para convite familiar e link público de divulgação;
- tela de convite acessível por `?convite=TOKEN`, sem expor dados da pessoa que enviou;
- ações de copiar o convite e compartilhar pelo WhatsApp;
- metadados Open Graph para compartilhamento da LiDire;
- proteção para que a tradução da interface não altere valores digitados pelo usuário.

O convite desta versão é uma camada de interface/fluxo do protótipo. A concessão real de acesso e a associação do membro familiar ao D1 continuam dependendo da etapa de backend correspondente.


## Versão 4.6 — compartilhamento social robusto
- Prévia social processada pelo Cloudflare Worker na borda.
- Links `?convite=` usam título, descrição e banner específicos de convite familiar.
- Links públicos usam a prévia institucional.
- Rotas diretas para as imagens sociais com `Content-Type: image/png` e cache público controlado.
- Metadados Open Graph/Twitter usam URLs absolutas do domínio atual.
- Arquivo `logo-lidire-oficial.png` permanece inalterado.


## LiDire MVP 4.7 — compartilhamento social

A 4.7 separa as páginas de compartilhamento das telas da SPA.
- `/convite?token=...` é uma página HTML dedicada ao crawler, com `og:image` absoluto e banner de convite.
- `/divulgacao` é a página pública de compartilhamento institucional.
- As imagens PNG são servidas por rotas explícitas do Worker com `Content-Type: image/png`.
- O convite copiado pelo aplicativo usa `/convite?token=...` e o botão da página de compartilhamento leva para `/?convite=...`.
- O nome do Worker no `wrangler.toml` foi atualizado para `mvp-versao-4-7`.
## LiDire MVP 5.2 — IA LiDire — convite visual e idioma

A 4.8 corrige o fluxo visual da página de convite familiar:
- O banner `lidire-invite-preview.png` passa a aparecer em tamanho grande e responsivo dentro da página aberta pelo convidado.
- O banner mantém proporção 1200×630 e a identidade visual oficial.
- A página de convite usa textos completos e consistentes com o idioma selecionado, sem combinar trechos de Português e English na mesma interface.
- O `logo-lidire-oficial.png` continua preservado sem alteração.
- O compartilhamento social da 4.7 permanece separado e funcional.



### v5.2 — convite ampliado e idioma consistente
- Aumenta a área visual do banner na página de convite, mantendo proporção 1200×630 e responsividade.
- O idioma do convite passa a acompanhar o idioma configurado por quem gera o convite, gravado no token de forma explícita.
- A página de compartilhamento `/convite` do Worker interpreta o idioma do token e entrega HTML, título, descrição e botão no idioma correspondente.
- Mantém a solução de compartilhamento social da v4.7/v4.8 e o logo oficial sem alterações.


## IA LiDire — primeira integração real

A 5.2 agora inclui uma integração real do Assistente LiDire com o Gemini, mantendo a chave de API exclusivamente no Cloudflare Worker. O Assistente envia uma versão sanitizada do contexto da rotina do usuário (tarefas, agenda, compras, estudos, treinos, hidratação, alimentação, finanças, objetivos, família e lembretes) para produzir respostas personalizadas. Dados como senha, sessão, endereço, telefone, fotos e outros segredos não são enviados ao modelo. O contexto do ciclo menstrual só é incluído quando a opção de uso pela IA estiver habilitada nas preferências.

### Configuração do Gemini

1. Crie/obtenha sua chave do Gemini no Google AI Studio.
2. Na pasta do projeto, configure a chave como segredo do Worker:

```bash
wrangler secret put GEMINI_API_KEY
```

3. Faça o deploy:

```bash
wrangler deploy
```

O modelo padrão configurado no `wrangler.toml` é `gemini-3.8-flash`, mas pode ser alterado pela variável `GEMINI_MODEL`. A chave **não deve** ser colocada no `app.js`, no HTML ou em qualquer arquivo público.

### O que a IA já faz

- conversa por texto dentro do Assistente;
- recebe o contexto dos módulos da LiDire;
- mantém as últimas interações da conversa durante a sessão;
- responde em Português ou English conforme o idioma configurado;
- usa também o reconhecimento de voz existente: a fala vira texto e é enviada à IA;
- pode ler a resposta em voz alta usando a síntese de voz do navegador.

### Próxima evolução

A base está preparada para a próxima etapa: permitir que a IA proponha ações estruturadas (por exemplo, criar uma tarefa, montar uma lista de compras ou organizar um plano de estudos) antes de executar qualquer alteração nos dados do usuário.


## MVP 5.2 — Identidade sonora
A versão 5.2 inclui uma assinatura sonora curta da LiDire para lembretes ativos no aplicativo e QR Code para a página pública de instalação/divulgação.

### Correções de estabilidade e UX — 5.2
- Internacionalização revisada com tradução bidirecional de telas, modais, botões e estados secundários, evitando substituições de palavras que corrompam textos.
- Removidos dados demonstrativos de compromissos familiares conhecidos de versões anteriores.
- Integrações familiares iniciam desconectadas e recursos compartilhados iniciam desativados até ação do usuário.
- Plano exibido como gratuito enquanto não houver fluxo real de assinatura.
- Fluxo Família → Configurações separado do fluxo Adicionar membro.
- QR Code e link de instalação disponíveis dentro do fluxo de Família.
- Histórico do ciclo e ações principais do ciclo tornados funcionais.
- Tipografia geral ampliada e contraste adicional no tema claro.

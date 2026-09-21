# Dossiê investigativo

## Arquitetura

- `dossie (6).html` é uma aplicação web estática de página única, com HTML, CSS e JavaScript no mesmo arquivo. A identidade visual é apresentada como **Dossiê da Harumi**, com uma direção editorial investigativa de alto contraste, tipografia expressiva e elementos de interface inspirados em arquivos confidenciais.
- `database.rules.json` contém as regras recomendadas para publicação no Firebase Realtime Database.
- O estado do dossiê (`sections`, itens, listas, imagens e quadro de investigação) é mantido em memória e possui uma cópia local por usuário via `localStorage`.
- A interface permite criar, editar, ordenar, importar e exportar o conteúdo do dossiê. O painel superior (hero) exibe a contagem de itens catalogados referente à seção ativa no momento. O quadro de investigação é um mural escalável, com controles de zoom, enquadramento e área navegável: o scroll sobre o quadro aproxima ou afasta as notas, enquanto as barras de rolagem permitem explorar o conteúdo ampliado. A pessoa também pode ajustar livremente a altura do mural.

## Persistência e acesso

- O Firebase será carregado como módulos ES do CDN oficial.
- O acesso é autenticado com e-mail e senha do Firebase Authentication. A sessão usa persistência local do navegador, mantendo a pessoa conectada até que ela escolha sair. Cada conta tem um dossiê isolado em `users/{uid}/dossier` no Firebase Realtime Database.
- O Firebase é a fonte de dados da conta autenticada. A cópia local é separada por usuário e serve apenas para contingência/offline; dados legados deste navegador só podem ser migrados mediante confirmação. Os dados recebidos são normalizados antes da renderização para manter compatibilidade com registros antigos. Alterações locais são renderizadas imediatamente e protegidas contra retornos remotos desatualizados durante a gravação.
- Imagens são reduzidas e armazenadas como Data URLs no JSON do dossiê, tanto na cópia local quanto no Realtime Database.
- O botão de login/logout e o estado de sincronização ficam na barra lateral.

## Configuração necessária no Firebase Console

- Habilitar o provedor **E-mail/senha** em Authentication.
- Aplicar regras do Realtime Database que só permitam acesso ao caminho `users/{uid}` pelo próprio usuário.

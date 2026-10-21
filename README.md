# Pantanal Vivo · Laboratório colaborativo DPSIR

**Aplicativo:** https://ernandes-sobreira.github.io/curso-carbonautas/

Plataforma científica e minimalista para construir coletivamente uma rede causal do Pantanal, com pesos de influência de **0 a 100**, sinal positivo/negativo e organização pelo método **DPSIR**.

## Salas independentes e persistentes

O endereço principal abre uma página de salas. A pessoa com a chave do **facilitador** pode criar uma turma, escolher o nome e receber um **código aleatório de oito caracteres**. Compartilhe **somente o código** com o grupo: o participante entra digitando-o em *Entrar com um código*, sem cadastro. Também é possível compartilhar o link `?s=CÓDIGO` ou o QR Code gerado dentro da sala.

- **Cada sala tem seus próprios conceitos, setas, forças, categorias DPSIR e gráficos.** Dados de outra turma não se misturam.
- Alterações feitas pelo grupo são **salvas automaticamente no banco Supabase**, não apenas no celular e não só quando alguém baixa o JSON.
- A sala pode ser reaberta outro dia com o mesmo código; as relações continuam disponíveis.
- O professor abre **Salas → Ver todas as minhas salas** e informa a mesma chave de facilitador usada na oficina principal; a lista é consultada no servidor e não depende de o celular lembrar os códigos.
- **Salas recentes** aparecem no aparelho em que foram acessadas. Essa lista local é somente um atalho, não é onde os dados da rede ficam guardados.
- Arquivos JSON e **Salvar versão** são cópias de segurança e registros de etapas: não são necessários para a persistência básica.
- A oficina antiga continua acessível em **Abrir oficina principal (legado)**; os conceitos existentes não são apagados.
- Não coloque a chave de facilitador dentro do QR Code dos alunos. No navegador do professor, você pode optar por lembrar a chave; evite isso em aparelhos compartilhados.

**Observação de segurança:** o código de oito caracteres funciona como convite de entrada; qualquer pessoa que conheça esse código pode editar a sala enquanto ela estiver liberada. Para dados sensíveis, é necessário autenticação individual, que esta oficina não implementa.

**Atenção à hospedagem gratuita:** o GitHub Pages hospeda apenas a interface; os dados persistem no Supabase e dependem da manutenção desse projeto. Planos gratuitos podem pausar por inatividade. Exporte JSON periodicamente para ter cópia independente do serviço.

## Funcionalidades

- **Rede:** conceitos e ligações direcionadas editáveis por todos os participantes da sala; entrada compartilhada via QR Code.
- **Matriz:** representação de influências assinadas de −100 a +100 (linha = origem; coluna = destino); `0` registrado é diferente de relação ausente.
- **DPSIR:** forças motrizes, pressões, estado, impactos e respostas, com possibilidade de retroalimentações.
- **Resultados:** gráficos de barras das dez ligações mais fortes e mais fracas; filtros positivas, negativas e peso zero; contagem e média das forças; ranking de conceitos pela soma das intensidades incidentes.
- **Arquivos:** exportação JSON completo, CSV dos conceitos, das ligações e da matriz, além de PNG da rede.
- **Importação integral:** abrir um arquivo JSON e reconstruir **a Rede, a Matriz assinada, o DPSIR e os gráficos da aba Resultados** com os dados desse arquivo. A prévia não altera o conteúdo da sala ao vivo; para passar à edição coletiva, o facilitador usa **Restaurar na sala**.
- **Versões arquivadas:** facilitador guarda versões no servidor; os participantes podem visualizá-las na aba Resultados. Ao restaurar um arquivo, um backup da rede anterior é criado automaticamente.
- **Sobre:** explicações metodológicas e limites científicos.

## Como conduzir a oficina

1. Abra o endereço principal, crie uma sala com a chave do facilitador e informe o código aos alunos. Projete o QR Code **dentro dessa sala**, ou peça que digitem o código na página inicial.
2. Cada pessoa adiciona palavras ou expressões e indica D/P/S/I/R.
3. Selecione origem e destino; defina a **intensidade 0–100** e o **sinal**. Edição e exclusão têm confirmação.
4. Discuta relações e hipóteses coletivas, consultando as abas **Matriz**, **DPSIR** e **Resultados**.
5. Ao terminar, clique em **Exportar → Projeto completo (.json)**. Esse arquivo permite reabrir a oficina posteriormente.
6. Para recuperar um projeto, use **Resultados → Importar JSON**. A plataforma abre a **aba Rede** com todos os nós e arestas do arquivo; visite **Matriz**, **DPSIR** e **Resultados** para conferir a análise completa. Uma faixa verde visível em todas as abas indica que você está na pré-visualização, sem alterar a sala ao vivo. Clique em **Voltar à sala ao vivo** para descartar a prévia, ou entre como **Facilitador** e selecione **Restaurar na sala** para substituir o trabalho atual (com backup automático).
7. Para manter o histórico sem trocar de arquivo, entre em **Facilitador** e escolha **Resultados → Salvar versão**; as versões ficam listadas nessa aba.

**Importante:** visitantes com o link público podem editar a rede, porque essa é uma oficina deliberadamente coletiva. Apenas o facilitador pode bloquear edições, reiniciar a sala, salvar versões ou restaurar um JSON. Não publique a chave privada de facilitador.

## Interpretação científica

O sinal é **causal-direcional**, não julgamento de valor: positivo (+) significa que um aumento na origem tende a aumentar o destino; negativo (−), que tende a diminuí-lo. A intensidade é uma avaliação da turma, **não um parâmetro físico medido**. Para um mapa cognitivo fuzzy, o peso assinado pode ser normalizado em −1 a +1 por `sinal × intensidade / 100`.

Os gráficos classificam os pesos atribuídos pelas pessoas, não correlações calculadas. O ranking de centralidade baseado na soma de intensidades incidentes é uma métrica exploratória, não um índice de causalidade ou importância ecológica validado.

## Hospedagem e dados

- **GitHub Pages:** `index.html` na raiz, com implantação automática via GitHub Actions em `.github/workflows/pages.yml`; home contém o gerenciador de salas.
- **Interface:** HTML/CSS/JavaScript com Cytoscape.js (visualização) e QRCode.js.
- **Persistência compartilhada:** serviço Supabase da oficina Pantanal Vivo, vinculado à sala atual.
- **Sincronização:** notificações Broadcast e atualização periódica de 7 segundos.
- **Arquivo e recuperação:** `pantdpsir_snapshots` com versões completas, acesso controlado na Edge Function `pantdpsir`. A restauração usa transação atômica e faz backup automático.
- **Privacidade:** a plataforma usa nome/apelido opcional nas contribuições; evite registrar dados sensíveis.

Se o GitHub Pages ainda não estiver configurado para este repositório vazio, entre em **Settings → Pages → Build and deployment → Source: GitHub Actions** e execute o workflow **Publicar Pantanal Vivo** na aba Actions. Feito isso, os pushes para `main` disparam novas publicações.

## Formato do arquivo JSON

O JSON preserva os **nós** (`nodes`), com IDs, rótulos, categorias DPSIR, coordenadas da rede e autoria, e as **ligações** (`edges`), com IDs de origem/destino, intensidade normalizada entre 0 e 1, sinal, justificativa e autoria. A plataforma reconstrói a **rede visual e sua matriz de adjacência assinada** a partir desses dados, sem necessidade de guardar uma segunda matriz redundante. Um arquivo JSON exportado pode ser importado novamente e analisado integralmente; durante a pré-visualização, a edição é bloqueada para não modificar a sala por engano.

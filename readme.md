# TECNOLOGIAS LEADZ - CMSOUZA

[  I. ESCOPO](https://github.com/leadzsoftware/cmsouza#i-escopo-desta-documenta%C3%A7%C3%A3o)<br />
[ II. PORTAIS](https://github.com/leadzsoftware/cmsouza#ii-portais)<br />
[III. SITE](https://github.com/leadzsoftware/cmsouza#iii-site)<br />
[ IV. CHATBOT](https://github.com/leadzsoftware/cmsouza#iv-chatbot)<br />
[  V. BACKUP - IMÓVEIS](https://github.com/leadzsoftware/cmsouza#v-backup---c%C3%B3pia-do-banco-de-im%C3%B3veis)<br />
[ VI. OTIMIZAÇÃO - IMAGENS](https://github.com/leadzsoftware/cmsouza#vi-c%C3%B3pia-e-otimiza%C3%A7%C3%A3o-das-imagens-dos-im%C3%B3veis)<br />
[VII. MÍDIAS DE ORIGEM](https://github.com/leadzsoftware/cmsouza?tab=readme-ov-file#vii-m%C3%ADdias-de-origem)<br />
[VIII. SUPORTE](https://github.com/leadzsoftware/cmsouza#vii-suporte)<br />
[ IX. NOTAS](https://github.com/leadzsoftware/cmsouza#notas)<br />


--------

# I. ESCOPO DESTA DOCUMENTAÇÃO

Detalha o funcionamento e configuração da integração dos portais de imóveis com o Vista[^vista], campos de entrada de lead através do site, funcionamento e integração do chatbot[^chatbot], backup do banco de dados dos imóveis e do funcionamento da otimização das imagens dos imóveis para o CDN da Leadz[^cdn].



# II. PORTAIS

A integração com os portais com a roleta de atendimento do Vista[^vista] funciona configurando o envio de lead do portal para o e-mail "cmsouza@cmslead.com.br".[^cmslead] <br />
Uma vez que o servidor Leadz recebe o e-mail, ele o interpreta e extrai as informações necessárias para cadastrar o lead na roleta. <br />
Exemplo de lead interpretado e enviado para o Vista[^vista]:

    agencia: 1
    veiculo: GrupoZap
    mensagem:  --conteúdo do e-mail--  
    nome: --nome do contato--
    fone: (43) 99###-####
    email: e-mail do contato
    anuncio: 7230
    interesse: locação
    departamento: 12

A informação "departamento" segue a seguinte ordem:

    CMSOUZA VENDA: 11
    CMSOUZA LOCAÇÃO: 12
    CMSOUZA LANÇAMENTOS: 13 (Para Meta Ads: envia o lead por e-mail para leads@cmsouza.com.br)

Os portais integrados são informados ao Vista[^vista] com as seguintes mídias de origem, sem espaços:

- Digital_GrupoZap
- Digital_VivaReal
- Digital_ChavesNaMão

Caso seja necessária a integração de um novo portal, favor fazer a solicitação através de um ticket de atendimento.



# III. SITE

As entradas de lead através do site são as seguintes:

- Página do imóvel - Quero mais informações:

As solicitações de contato entram para a roleta de atendimento do Vista[^vista] com a mídia de origem definida como "Digital_Site", contendo o Nome, Telefone e Imóvel de Interesse.

- Barra de contatos:

Os contatos provenientes da opção "Atendimento por WhatsApp" na barra de contatos redirecionam o cliente para o atendimento através do chatbot[^chatbot] (detalhado na seção seguinte), entram para a roleta de atendimento do Vista[^vista] com a mídia de origem definida como "Digital_Site_Whatsapp", e com a mensagem enviada pelo cliente incluída no campo "mensagem".

- Configurando os imóveis na categoria "Exclusividade Vectra Construtora":

É necessário que o EMPREENDIMENTO esteja cadastrado no CRM na categoria **EMPREENDIMENTO** e com os checkboxes **"Exibir no Site"** e **"Exclusividade"** marcados. Os **EMPREENDIMENTOS** não são exibidos na pesquisa de **IMÓVEIS.**
- [x] Exibir no Site
- [x] Lançamento

- Configurando os imóveis na categoria "Desenvolvimento Real Estate Intelligence":

É necessário que o EMPREENDIMENTO esteja cadastrado no CRM na categoria **EMPREENDIMENTO** e com os checkboxes **"Exibir no Site"** e **"BTS"** marcados. Os **EMPREENDIMENTOS** não são exibidos na pesquisa de **IMÓVEIS.**
- [x] Exibir no Site
- [x] BTS

### Sobre as páginas de cadastro

- Trabalhe Conosco[^trabalhe]:

O cadastro do currículo captado pela página é enviado para o e-mail "contato@cmsouza.com.br" com os seguintes dados:
Assunto: Novo Currículo
As informações de Nome, E-mail e Telefone vão no corpo do e-mail com o currículo anexo

- Anuncie seu Imóvel[^anuncie]:

As informações captadas nesta página são direcionadas à roleta de atendimento e direcionado ao departamento de acordo com o interesse selecionado. Este contato vai com a mensagem "Gostaria de anunciar meu imóvel no seu site." e com a mídia de origem "Anuncie seu Imóvel - Site" cadastrada no lead.


# IV. CHATBOT

Nosso sistema (Leadz) atua em conjunto com o serviço contratado "Sendpulse"[^chatbot], utilizando o chatbot como forma de entrada de dados. O chatbot faz a interação com o usuário durante o atendimento dos leads no WhatsApp, onde posteriormente nosso sistema os envia para a roleta de atendimento do Vista[^vista] com a mídia de origem "Site-Whatsapp". <br />
Para os contatos provenientes dos portais, nosso sistema identifica os links dentro das mensagens e classifica a mídia de origem de acordo com o link do portal.
Dessa forma, os leads dos portais atendidos através do WhatsApp são enviados para a roleta de atendimento com uma das seguintes mídias de origem:

    Digital_GrupoZap_Whatsapp
    Digital_VivaReal_Whatsapp
    Digital_ChavesNaMão_Whatsapp

Os outros contatos provenientes da opção "Atendimento por WhatsApp" da barra de contatos do site da CMSouza são classificados com a mídia de origem "Site-Whatsapp". <br />
O nosso sistema, em conjunto com o chatbot[^chatbot], armazena as mensagens recebidas durante a interação do chat e ao final as envia no campo "mensagem" do lead a ser cadastrado na roleta de atendimento.

O fluxo de atendimento quando não tem um código de imóvel informado funciona da seguinte forma:

    1. Início do atendimento e interpretação da primeira mensagem enviada pelo usuário.
    2. O servidor inicia o fluxo de perguntas para o usuário de acordo com a primeira mensagem
    3. Usuário seleciona a finalidade do atendimento, que indica a informação do departamento
    4. O processamento dos dados é iniciado assim que o servidor recebe a resposta do "nome"
    5. Após, o nosso sistema faz o envio do lead para a roleta do Vista

Diagrama:

```mermaid
stateDiagram
    
    1: IA - Identificação
    1: Nome
    1: Telefone
    2: Seleção de Finalidade
    2: Corretor de
    2: Venda ou
    2: Locação
    2: Administrativo
    3: Processamento dos Dados
    4: Envio do Lead
    direction tb
    
    1 --> 2
    2 --> 3
    3 --> 4
```

Exemplo de lead enviado através do chatbot:

```
nome: --nome do contato--
fone: 5543#####
mensagem: Olá! Estou no site da CMSouza e gostaria de mais informações.
veiculo: Site-Whatsapp
interesse: Locação
departamento: 12
agencia:1
```

*O imóvel de interesse, neste caso, não é enviado ao o Vista[^vista]. <br />

O fluxo de atendimento no caso de ter um código de imóvel informado funciona da seguinte forma:

    1. Início do atendimento
    2. O servidor interpreta a mensagem enviada pelo cliente e identifica o código do imóvel informado
    3. O servidor capta automaticamente o nome e telefone do contato
    4. O servidor faz uma pesquisa ao CRM para identificar o imóvel e a finalidade (Venda ou Locação)
    5. Após, o nosso sistema faz o envio do lead para a roleta do Vista

Diagrama:

```mermaid
stateDiagram
    
    1: IA - Identificação
    1: Nome
    1: Telefone
    1: Imóvel de interesse
    2: Pesquisa no CRM
    3: Identificação de finalidade
    v: Venda
    l: Locação
    vl: Venda ou Locação
    4: Processamento dos Dados
    5: Envio do Lead
    direction tb
    
    1 --> 2
    2 --> 3
    state 3 {
    state fork_state <<fork>>
    direction TB
        fork_state
        fork_state --> v
        fork_state --> vl
        fork_state --> l
    state join_state <<join>>
    v --> join_state
    vl --> join_state
    l --> join_state
    }
    3 --> 4
    4 --> 5
```
Exemplo de lead enviado através do chatbot com imóvel reconhecido:

```
nome: --nome do contato--
fone: 5543#####
mensagem: Olá! Gostaria de mais informações sobre o imóvel 1234.
veiculo: Site-Whatsapp
anuncio: 1234
interesse: Locação
departamento: 12
agencia:1
```

# V. BACKUP - CÓPIA DO BANCO DE IMÓVEIS

A cópia/backup do banco de imóveis salva apenas as informações que são pertinentes ao funcionamento do site. O backup não salva o banco de dados dos imóveis em sua totalidade e não tem acesso à outras informações do CRM NovoVista[^vista], como por exemplo, cadastro de clientes e negócios. <br />
O intuito dessa cópia é para que o site não dependa do sistema do Novo Vista[^vista], que esporadicamente apresenta instabilidades e lentidão. A cópia ocorre todos os dias, de hora em hora e leva cerca de 15 minutos para finalizar e atualizar. Portanto, as atualizações de descrição e o cadastro de novos imóveis pode levar até uma hora para estarem disponíveis no site da CMSouza.

As informações dos imóveis que são salvas são as seguintes:

    Status, Finalidade, Categoria, Codigo, BairroComercial, Bairro, Empreendimento, Cidade, 
    ValorVenda, ValorLocacao, Dormitorios, Caracteristicas, InfraEstrutura, CodigoEmpreendimento,
    BanheiroSocialQtd, Vagas, AreaPrivativa, AreaTotal, ValorCondominio, DescricaoWeb, 
    DescricaoEmpreendimento, Imediacoes, DestaqueWeb, SuperDestaqueWeb, Lancamento, Tour360, 
    Corretor, TotalBanheiros, Suites, Endereco, Numero, UF, FotoDestaque



# VI. CÓPIA E OTIMIZAÇÃO DAS IMAGENS DOS IMÓVEIS

As fotos dos imóveis também passam por uma otimização em nosso servidor, e, posteriormente, é replicada para o nosso CDN[^cdn]. <br />
Com isso, reduzimos o tempo médio de carregamento de cada imagem do imóvel em aproximadamente 80%, indo de ~300ms[^~] [^ms] para cerca de ~60ms[^ms]. Isso equivale a um carregamento de imagens 4 vezes mais rápida em comparação ao CDN[^cdn] do Vista[^vista]. <br />

# VII. MÍDIAS DE ORIGEM

Segue a nomenclatura




# VIII. ESTRUTURA DE CÓDIGOS PARA IDENTIFICAÇÃO DE EMPREENDIMENTOS

Segue o padrão de nomenclatura **LLNNNNN**

**LL**: Duas letras indicando a mídia de captação.
**NN**: Dois primeiros números indicando a construtora.
**NNN**: Três últimos números indicando o empreendimento.

Mídia de Captação (LL):

ME: Meta
LP: Landing Page
RD: RDStation

Construtora (NN - 01 a 99):

01 - Vectra
02 - Paysage
03 - Zacaria
04 - Artesano

Empreendimento (NN - 01 a 999):
Numerados de 01 a 999 para cada construtora.

    01 Vectra
        001 - Hera
    02 Construtora Zacaria
        001 - Maison Jardin
    03 Paysage Corpal
        001 - Haus
        002 - Garnet
    04 Artesano
        001 - Artesano

Exemplo de uso:

**ME01001**: MetaAds, Vectra, Hera
**LP03002**: LandingPage, Paysage Corpal, Garnet

**Implementação nas Mensagens**:

O código gerado deve ser incluído nos final dos textos das mensagens pré-definidas para o WhatsApp nas campanhas ativas pela equipe responsável. Isso permite que o sistema interprete os códigos e faça o direcionamento adequado



# .IX SUPORTE

Via ticket de atendimento, para [cmsouza@leadzsoftware.zohodesk.com](mailto:cmsouza@leadzsoftware.zohodesk.com) <br />

Ou e-mail: [suporte@leadz.software](mailto:suporte@leadz.software) <br />

### Data de Criação
01/10/2022 <br />

### Última Atualização
10/06/2024 <br />

### Histórico - Link anterior
https://github.com/pepeleascov/cmsouza <br />

#### IX. NOTAS
[^chatbot]: [Chatbot - Sendpulse](https://sendpulse.com)
[^cdn]: CDN - Content Delivery Network / Rede de entrega de conteúdo
[^cmslead]: CMSLEAD - Sistema criado para recebimento e interpretação dos e-mails de portais
[^ms]: milisegundos
[^~]: ∼ (aproximação, similaridade, equipolência)
[^vista]: [NovoVista](http://www.vistasoft.com.br/)
[^trabalhe]: [Trabalhe Conosco](https://www.cmsouza.com.br/trabalhe-conosco)
[^anuncie]: [Anuncie seu imóvel](https://www.cmsouza.com.br/anuncie-seu-imovel) 

<!-- 
Comment
-->